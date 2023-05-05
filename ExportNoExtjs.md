Como ultilizar Php Excel;
https://github.com/PHPOffice/PHPExcel Baixe esse código e bote dentro do seu projeto ou fora precisamos apenas chamar o arquivo:

Depois de criamos observe como eu fiz:
![image](https://user-images.githubusercontent.com/87372927/236457244-8acdca7f-1ab0-46a1-a7c5-d6b44925815a.png)
Botei dentro da pasta do PhpExcel uma Pasta chamada testeBruna
Dentro dessa pasta criei um arquivo um exemplos com pesquisa ele vai exporta as informações com pesquisa na tela se não quiser exportar tudo é só tirar o if/else
Com pesquisa Observe:
<?php
//Incluir arquivo
require('../Classes/PHPExcel.php');
 
$conexao = new PDO("mysql:host=127.0.0.1;dbname=controle_treinamento",'root', '');

if(empty($_GET['searchNome'])):
  $informacao = '';
else:
  $informacao = $_GET['searchNome'];
endif;  
//print_r($informacao);die;

if(!empty($informacao)):
  $dados = $conexao->prepare("SELECT *  FROM funcionarios WHERE  nome LIKE '%{$informacao}%'");	

else:
  
  $dados = $conexao->prepare( "SELECT * FROM funcionarios");	
endif;

//print_r($dados);die;



//Criando o Objeto PHPExcel
$objPHPExcel = new PHPExcel();
//Propriedades do PHP
$objPHPExcel->getProperties()->setCreator('Bruna Vitoria')
                             ->setTitle('Estudando PHP Excel')
                             ->setLastModifiedBy("Modificado por Bruna")
                             ->setSubject("PHP")
                             ->setDescription("Descrição")
                             ->setKeywords("Key")
                             ->setCategory("Categoria");

//fonte no excel
$objPHPExcel->getDefaultStyle()->getFont()->setName('Arial')
                                          ->setSize(14);

//Define a largura das colunas de modo automático
			$objPHPExcel->getActiveSheet()->getColumnDimension('A')->setWidth(5);
			$objPHPExcel->getActiveSheet()->getColumnDimension('B')->setAutoSize(true);
            $objPHPExcel->getActiveSheet()->getColumnDimension('C')->setAutoSize(true);
            $objPHPExcel->getActiveSheet()->getColumnDimension('D')->setAutoSize(true);
            $objPHPExcel->getActiveSheet()->getColumnDimension('E')->setAutoSize(true);
       
     $objPHPExcel->getActiveSheet()->mergeCells('A1:E1')->setCellValue("A1","Gerenciamento de Informações (CLIENTE)");   
     $objPHPExcel->getActiveSheet()->getStyle('A1')->getAlignment()->setHorizontal('center');                                      
//Colunas 

$objPHPExcel->getActiveSheet() ->setCellValue('A2','ID')
                               ->setCellValue('B2','Nome')
                               ->setCellValue('C2','Admissao')
                               ->setCellValue('D2','Situacao')
                               ->setCellValue('E2', 'Cargo');
// Centraizando colunas acima 
$objPHPExcel->getActiveSheet()->getStyle('A2:E2')->getAlignment()->setHorizontal('center');                               
// Bordas das colunas acima
$objPHPExcel->getActiveSheet()->getStyle('A2:E2')->applyFromArray(
    array(
        'font' => array(
            'size' => 18,
            'bold' => true,
        ),
        'borders' => array(
            'allborders' => array(
                'style' => PHPExcel_Style_Border::BORDER_THIN
            )
        )
    )
            );

$dados->execute();
//Inserindo dados nas células
//print_r($dados);die;

$cont = 3; 
foreach($dados as $valor){    
  //print_r($valor);die;
    $objPHPExcel->getActiveSheet() ->setCellValue('A' . $cont, $valor['cod'])
                                   ->setCellValue('B' . $cont, $valor['nome'])
                                   ->setCellValue('C' . $cont, $valor['admissao'])
                                   ->setCellValue('D' . $cont, $valor['situacao'])
                                   ->setCellValue('E' . $cont, $valor['cargo']);   
                                                                   
 $objPHPExcel->getActiveSheet()->getStyle('A3:E'.$cont)->getAlignment()->setHorizontal('center');   //deixar todos centralizados  
 $cont++;  
                        
 }; 
 //print_r($valor);die;                       

//Nomeando a planilha
 $objPHPExcel->getActiveSheet()->setTitle('Funcionario'); 
 $objPHPExcel->setActiveSheetIndex(0); //O index do nome da planilha                           


 header('Content-Type: application/vnd.openxmlformats- officedocument.spreadsheetml.sheet');
 header('Content-Disposition: attachment;filename="clientefiltro.xls"');
 header('Cache-Control: max-age=0');
 //header("Content-type: application/force-download");  

$objWriter = PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel2007');
ob_end_clean();
$objWriter->save('php://output');
?>

Dentro do código chamei dentro do controller;
 init: function(application){
        this.control({
           "cadastros button#exportFun":{
              click: this.onExportFuncClick  
            },
        })
        
        onExportFuncClick: function (btn, e , eOpts){
        var searchNome = Ext.ComponentQuery.query('[itemId=searchNome]')[0].getValue();
       
        var win = Ext.create('Ext.window.Window', {
            title: 'Baixando arquivo Excel',
            width: 300,
            height: 200,
            modal: true,
            layout: 'fit',
            maximizable: true
        });
        href = 'PHPExcel/testeBruna/exceltelafun.php?&searchNome='+searchNome;
        win.html = '<iframe src="' + href + '" name="principal" width="100%" height="100%" scrolling="auto" frameborder="0"></iframe>';
        win.show();
    },
    
Dentro da view teremos o botão e campo de pesquisa:
  { 
                        xtype: 'button',
                        text:'Exportar',
                        itemId:'exportFun',
                        ui:'plain', // ele permite adcionar o style no button                 
                        style:'background-color: #008CBA; color: white;padding: 5px 10px;text-align: center;border-radius: 5px',
                        bodyPadding: 10,
                    },
                    {
                      flex: '1',
                    },
                    {
                        xtype: 'textfield',
                        name:'searchNome',
                        itemId:'searchNome',
                        labelWidth: 80,
                        fieldLabel: 'Pesquisar',
                        emptyText: 'Nome do Funcionario',
                        //Propriedade que muda o css do input
                        fieldStyle: ' margin-left: none; background-image: none; border-radius: 4px;padding: 2.5px 8px'
                    },


