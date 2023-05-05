Isso é um exemplo simples de php sem usar framework de back-end
Observação dentro de uma pasta php:
Tenho arquivos de php que foram chamados na store.
Crio uma pasta php dentro do projeto e boto os seguintes phps
conectarphp.php Conecta o Php com o banco de dados
<?php
 
//nome do servidor (127.0.0.1)
$servidor = "127.0.0.1";
 
//usuário do banco de dados
$user = "root";
 
//senha do banco de dados
$senha = "";
 
//nome da base de dados
$db = "controle_treinamento";
 
//executa a conexão com o banco, caso contrário mostra o erro ocorrido
$conexao = mysqli_connect($servidor,$user,$senha) or die (mysqli_error($conexao));

//seleciona a base de dados daquela conexão, caso contrário mostra o erro ocorrido
$banco = $conexao->select_db($db) or die(mysqli_error($conexao));
 
?>
listaphp.php Mostra as informaçoês no banco de dados;
Observação esse php tem exemplo de fazer pesquisa;
<?php
	// chama o arquivo de conexão com o bd
	include("conectar.php");

	$start = $_REQUEST['start'];
	$limit = $_REQUEST['limit'];
	$data1 = '';
	
    if(!empty($_GET['searchNome'])){
		$data1 .= "AND nome LIKE '%{$_GET['searchNome']}%'";
	}

	if(!empty($_GET['searchCargo'])){
		$data1 .= " AND  cargo LIKE '%{$_GET['searchCargo']}%'"; 
    }
	if(!empty($data1)){
		$queryString = "SELECT * FROM funcionarios WHERE 1=1  {$data1}  LIMIT $start,  $limit ";

	}  
	else{
	 	$queryString = "SELECT * FROM funcionarios WHERE 1=1  LIMIT $start,  $limit";	
	};
 
	//consulta sql
	$query = $conexao->query($queryString) or die(mysqli_error($conexao));

	//faz um looping e cria um array com os campos da consulta
	$funcionarios = array();
	while($funcionario = mysqli_fetch_assoc($query)) {
	    $funcionarios[] = $funcionario;
	}

	//consulta total de linhas na tabela
	$queryTotal = $conexao->query('SELECT count(*) as num FROM funcionarios') or die(mysqli_error($conexao));
	$row = mysqli_fetch_assoc($queryTotal);
	$total = $row['num'];

    ;
	//encoda para formato JSON
	echo json_encode(array(
		"success" => mysqli_errno($conexao) == 0,
		"total" => $total,
		"funcionarios" => $funcionarios
	));
?>

// Criar uma nova informação no banco de dados
criarphp.php

<?php
	//chama o arquivo de conexão com o bd
	include("conectar.php");

	$info = $_POST['funcionarios'];

	$data = json_decode(stripslashes($info));

	$nome = $data->nome;
	$admissao = $data->admissao;
	$situacao = $data->situacao;
	$cargo = $data->cargo;

	//consulta sql
	$query = sprintf("INSERT INTO funcionarios(nome, admissao, situacao, cargo) values ('%s', '%s', '%s', '%s' )",
	mysqli_real_escape_string($conexao, $nome),
	mysqli_real_escape_string($conexao, $admissao),
	mysqli_real_escape_string($conexao, $situacao),
	mysqli_real_escape_string($conexao, $cargo));

	$rs = $conexao->query($query);

	echo json_encode(array(
		"success" => mysqli_errno($conexao) == 0,
		"funcionarios" => array(    //Só esse nome é valido para o root do livro não precisa botar tudo com o mesmo nome só esse.
			"cod" => mysql_insert_id(),
			"nome" => $nome,
			"admissao" => $admissao,
			"situacao" => $situacao,
			"cargo" => $cargo
		)
	));
?>
// deleta informações do banco de dados
criar deletephp.php
<?php
	//chama o arquivo de conexão com o bd
	include("conectar.php");

	$info = $_POST['funcionarios'];

	$data = json_decode(stripslashes($info));

	$cod = $data->cod;

	//consulta sql
	$query = sprintf("DELETE FROM funcionarios WHERE cod=%d",
		mysqli_real_escape_string($conexao, $cod));

	$rs = $conexao->query($query);

	echo json_encode(array(
		"success" => mysqli_errno($conexao) == 0
	));
?>
// Atualiza informações do banco de dados, ou seja, modifica:

 <?php
	//chama o arquivo de conexão com o bd
	include("conectar.php");

	$info = $_POST['funcionarios'];

	$data = json_decode(stripslashes($info));

	$nome = $data->nome;
	$admissao = $data->admissao;
	$situacao = $data->situacao;
	$cargo = $data->cargo;
	$cod = $data->cod;

	//consulta sql
	$query = sprintf("UPDATE funcionarios SET nome = '%s', admissao = '%s', situacao = '%s', cargo = '%s'  WHERE cod=%d",
	mysqli_real_escape_string($conexao, $nome),
	mysqli_real_escape_string($conexao, $admissao),
	mysqli_real_escape_string($conexao, $situacao),
	mysqli_real_escape_string($conexao, $cargp),
    mysqli_real_escape_string($conexao, $cod));
	
	$rs = $conexao->query($query);

	echo json_encode(array(
		"success" => mysqli_errno($conexao) == 0,
		"funcionarios" => array(
			"cod" => $cod,
			"nome" => $nome,
			"admissao" => $admissao,
			"situacao" => $situacao,
			"cargo" => $cargo
		)
	));
?>


