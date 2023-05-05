Um exemplo grande de controler.

 Ext.define('ExtMVC.controller.Main', {
    extend: 'Ext.app.Controller',

    models: [ // Todos os models tem que ficar aqui
        'ExtMVC.model.Livro',
        'ExtMVC.model.Funcionario',
        'ExtMVC.model.ClienteVip',
        'ExtMVC.model.Usuario'

    ],
    stores: [  // Todas as stores tem que ficar aqui
        'ExtMVC.store.Livros',
        'ExtMVC.store.Funcionarios',
        'ExtMVC.store.ClientesVips',
        'ExtMVC.store.Usuarios'
    ],
    views: [ // todas as views tem que ficar aqui
        'ExtMVC.view.Viewport',
        'ExtMVC.view.LivrosGrid',
        'ExtMVC.view.FuncionariosGrid',
        'ExtMVC.view.FormLivro',
        'ExtMVC.view.ClientesGrid',
        'ExtMVC.view.TelaViewSub',
        'ExtMVC.view.Cadastrar',
        'ExtMVC.view.FormFuncionario',
        'ExtMVC.view.FormCliente',
        'ExtMVC.view.ImportCliente',
      //  'ExtMVC.view.WindowExport'


    ],
    init: function (application) {
        this.control({

            //LOGIN E CADASTRAR
            "myviewport button#entrar": {
                click: this.onLoginClick
            },
            "myviewport button#cadastrar": {
                click: this.onCadastrarClick
            },
            "cadastrarform button#cancelar": {
                click: this.onCancelarClickCad
            },
            "cadastrarform button#salvarCadastrar": {
                click: this.onSalvarClickCad
            },


            //CERUD LIVROSGRID
            "livrosgrid": {
                render: this.onGridRender,
                itemdblclick: this.onEditarClick
            },
            "livrosgrid button#add": {
                click: this.onAdicionarClick
            },
            "livrosgrid button#deletar": {
                click: this.onDeleteClick
            },
            "livroform button#cancelar": {
                click: this.onCancelarClick
            },
            "livroform button#salvar": {
                click: this.onSalvarClick
            },

            //CRUD FUNCIONARIOSGRID
            "funcionariosgrid": {
                render: this.onGridRenderFun,
                itemdblclick: this.onEditarClickFun
            },
            "funcionariosgrid button#add": {
                click: this.onAdicionarClickFun
            },
            "funcionariosgrid button#deletar": {
                click: this.onDeleteClickFun
            },
            "funcionarioform button#cancelar": {
                click: this.onCancelarClickFun
            },
            "funcionarioform button#salvar": {
                click: this.onSalvarClickFun
            },
            // CRUD CLIENTESVIP

            "clientesvip": {
                render: this.onGridRender,
                itemdblclick: this.onEditarClickCli
            },
            "clientesvip button#add": {
                click: this.onAdicionarClickCli
            },
            "clientesvip button#deletar": {
                click: this.onDeleteClickCli
            },
            "clienteform button#cancelar": {
                click: this.onCancelarClickCli
            },
            "clienteform button#salvar": {
                click: this.onSalvarClickCli
            },

            //IMPRIMIR

            "livrosgrid button#imprimir": {
                click: this.onImprimirClick
            },
            "funcionariosgrid button#imprimir": {
                click: this.onImprimirClickFun
            },
            "clientesvip button#imprimir": {
                click: this.onImprimirClickCli
            },
            //Pesquisas
            "livrosgrid button#pesquisar": {
                click: this.onPesquisarClickLiv
            },
            "funcionariosgrid button#pesquisar": {
                click: this.onPesquisarClickFun
            },
            "clientesvip button#pesquisar": {
                click: this.onPesquisarClickCli
            },
            //Export
            "clientesvip button#buttonExport": {
                click: this.onExportClickCli
            },
            "clientesvip button#exportTela": {
                click: this.onExportTelaCli
            },
            //Import

            "clientesvip button#buttonImport": {
                click: this.onViewImportClickCli
            },
            "importcliente button#ImportarCli": {
                click: this.onImportClickCli
            },
            "importcliente button#cancelar": {
                click: this.onCancelarImportCli
            },

        })
    },



    onLoginClick: function (btn, e, eOpts) {
        var store = Ext.getStore('ExtMVC.store.Usuarios')
        // var email = store.data.items[0].data.email // Percorrendo as informações da storie
        // var senha = store.data.items[0].data.senha // Já qu eos dois vem da storie chamei uma sub storie
        var subStore = store.data.items
        //    console.log(subStore)
        var form = btn.up('form#Cadastrar').getValues(); // Informações que vem do formulario
        var emailform = form.email;
        var senhaform = form.senha;
        var login; // para parar de rodar

        for (let index = 0; index < subStore.length; index++) { // Para percorrer tudo que tem do array
            var email = subStore[index].data.email;
            var senha = subStore[index].data.senha  //Chamando a sub storie com o resto do nome 


            if (emailform == email && senhaform == senha) {  // fazendo comparação
                var win = btn.up('viewport');
                win.getLayout().setActiveItem(1)
                Ext.MessageBox.alert('Bem Vindo', 'Bem Vindo ao sistema de informações!')
                return login = "sucess"; //Para o laço parar de rodar

            }
            else {
                login = 'false'
                console.log("Deu errado")
            }

        }
        if (login == "false") {
            Ext.MessageBox.alert('Alerta', 'Informações erradas!')
        }


    },
    onCadastrarClick: function (btn, e, eOpts) {
        var win = Ext.create('ExtMVC.view.Cadastrar');
        win.setTitle('Cadastrar Gerente')
    },
    onCancelarClickCad: function (btn, e, e0pts) {
        var win = btn.up('window');
        var form = win.down('form');
        form.getForm().reset();
        win.close();

    },
    onSalvarClickCad: function (btn, e, e0pts) {
        var win = btn.up('window#formCadastrar'),
            form = win.down('form#formularioCadastrar'),
            values = form.getValues()
        var store = Ext.getStore('ExtMVC.store.Usuarios')

        var usuario = Ext.create('ExtMVC.model.Usuario', {

            nome: values.nome,
            email: values.email,
            senha: values.senha,
            telefone: values.telefone
        });

        store.insert(0, usuario);
        Ext.MessageBox.show({
            title: 'Atenção!',
            msg: 'Usuário criado com sucesso',
            width: 400,
            icon: Ext.MessageBox.INFO
        })

        store.sync();

        win.close();

    },

    onGridRender: function (grid, eOpts) { // Livro
        grid.getStore().load();
    },
    onGridRenderFun: function (grid, eOpts) { // Funcionario
        grid.getStore().load();
    },
    onGridRendercli: function (grid, eOpts) { // Funcionario
        grid.getStore().load();
    },
    onAdicionarClick: function (btn, e, eOpts) {  //Livro
        var win = Ext.create('ExtMVC.view.FormLivro');
        win.setTitle('Adicionar Livro')

    },
    onAdicionarClickFun: function (btn, e, eOpts) { //Funcionario
        var win = Ext.create('ExtMVC.view.FormFuncionario');
        win.setTitle('Cadastrar Funcionario')

    },
    onAdicionarClickCli: function (btn, e, eOpts) { //Funcionario
        var win = Ext.create('ExtMVC.view.FormCliente');
        win.setTitle('Cadastrar Cliente')

    },
    onEditarClick: function (btn, record, item, index, e, eOpts) {
        var win = Ext.create('ExtMVC.view.FormLivro');
        win.setTitle('Editar - ' + record.get('nome'));
        var form = win.down('form#formulariolivro') //Dei atravez de item id botando primeiro o que ele é 
        form.loadRecord(record);
    },
    onEditarClickFun: function (btn, record, item, index, e, eOpts) {
        var win = Ext.create('ExtMVC.view.FormFuncionario');
        win.setTitle('Editar - ' + record.get('nome'));
        var form = win.down('form#formulariofuncionario') //Dei atravez de item id botando primeiro o que ele é 
        form.loadRecord(record);
    },
    onEditarClickCli: function (btn, record, item, index, e, eOpts) {
        var win = Ext.create('ExtMVC.view.FormCliente');
        win.setTitle('Editar - ' + record.get('nome'));
        var form = win.down('form#formulariocliente') //Dei atravez de item id botando primeiro o que ele é 
        form.loadRecord(record);


    },
    onDeleteClick: function (btn, e, eOpts) {
        var grid = btn.up('grid');
        var records = grid.getSelectionModel().getSelection(); // EStá funcionando
        var store = grid.getStore();
        Ext.MessageBox.confirm('Confirmacao', 'Quer mesmo deletar o registro?', function (btn) {
            if (btn == 'yes') {
                store.remove(records);
                store.sync();
                Ext.MessageBox.alert('Mensagem', 'Registro Deletado com sucesso')
            } else if (btn == 'no') {

            }
        })

    },
    onDeleteClickFun: function (btn, e, eOpts) {
        var grid = btn.up('grid');
        var records = grid.getSelectionModel().getSelection(); // EStá funcionando
        var store = grid.getStore();
        Ext.MessageBox.confirm('Confirmacao', 'Quer mesmo deletar o registro?', function (btn) {
            if (btn == 'yes') {
                store.remove(records);
                store.sync();
                Ext.MessageBox.alert('Mensagem', 'Registro Deletado com sucesso')
            } else if (btn == 'no') {
                console.log("Mensagem", "Não foi deletado")

            }
        })

    },
    onDeleteClickCli: function (btn, e, eOpts) {
        var grid = btn.up('grid');
        var records = grid.getSelectionModel().getSelection(); // EStá funcionando
        var store = grid.getStore();
        Ext.MessageBox.confirm('Confirmacao', 'Quer mesmo deletar o registro?', function (btn) {
            if (btn == 'yes') {
                store.remove(records);
                store.sync();
                Ext.MessageBox.alert('Mensagem', 'Registro Deletado com sucesso')
            } else if (btn == 'no') {
                console.log("Mensagem", "Não foi deletado")

            }
        })

    },
    onCancelarClick: function (btn, e, eOpts) {
        var win = btn.up('window');
        var form = win.down('form');
        form.getForm().reset();
        win.close();

    },
    onCancelarClickFun: function (btn, e, eOpts) {
        var win = btn.up('window');
        var form = win.down('form');
        form.getForm().reset();
        win.close();

    },
    onCancelarClickCli: function (btn, e, eOpts) {
        var win = btn.up('window');
        var form = win.down('form');
        form.getForm().reset();
        win.close();

    },
    onSalvarClick: function (btn, e, eOpts) {
        var win = btn.up('window#WindowId'),
            form = win.down('form#formulariolivro'),
            values = form.getValues(),
            record = form.getRecord(),
            grid = Ext.ComponentQuery.query('livrosgrid')[0],
            store = grid.getStore();

        if (record) {

            record.set(values);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Informação alterada com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })

        } else {

            var livros = Ext.create('ExtMVC.model.Livro', {

                nome: values.nome,
                quantidade: values.quantidade,
                genero: values.genero,
                preco: values.preco,
                precoVip: values.precoVip


            });

            store.insert(0, livros);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Produto criado com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })
        }

        store.sync();

        win.close();
    },
    onSalvarClickFun: function (btn, e, eOpts) {
        var win = btn.up('window#WindowId2'),
            form = win.down('form#formulariofuncionario'),
            values = form.getValues(),
            record = form.getRecord(),
            grid = Ext.ComponentQuery.query('funcionariosgrid')[0],
            store = grid.getStore();

        if (record) {

            record.set(values);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Informação alterada com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })

        } else {

            var funcionarios = Ext.create('ExtMVC.model.Funcionario', {

                nome: values.nome,
                email: values.email,
                cargo: values.cargo,
                telefone: values.telefone,
                salario: values.salario


            });

            store.insert(0, funcionarios);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Funcionario criado com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })

        }

        store.sync();

        win.close();
    },
    onSalvarClickCli: function (btn, e, eOpts) {
        var win = btn.up('window#WindowId3'),
            form = win.down('form#formulariocliente'),
            values = form.getValues(),
            record = form.getRecord(),
            grid = Ext.ComponentQuery.query('clientesvip')[0],
            store = grid.getStore();

        if (record) {

            record.set(values);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Informação alterada com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })

        } else {

            var clientes = Ext.create('ExtMVC.model.ClienteVip', {

                nome: values.nome,
                email: values.email,
                cpf: values.cpf,
                telefone: values.telefone,


            });

            store.insert(0, clientes);
            Ext.MessageBox.show({
                title: 'Atenção!',
                msg: 'Cliente criado com sucesso',
                width: 400,
                icon: Ext.MessageBox.INFO
            })
        }

        store.sync();

        win.close();
    },

    onImprimirClick: function (btn, e, eOpts) {
        window.open('resources/imprimir/imprimirLivros.php')
    },
    onImprimirClickFun: function (btn, e, eOpts) {
        window.open('resources/imprimir/imprimirFuncionarios.php')
    },
    onImprimirClickCli: function (btn, e, eOpts) {

        window.open('resources/imprimir/imprimirClientes.php')

    },
    onPesquisarClickLiv: function (btn, e, eOpts) {
        var campoNome = Ext.ComponentQuery.query('[itemId=campoNome]')[0].getValue();
        var campoGenero = Ext.ComponentQuery.query('[itemId=campoGenero]')[0].getValue();
        var campoPreco = Ext.ComponentQuery.query('[itemId=campoPreco]')[0].getValue();
        var grid = btn.up('grid')
        var store = grid.getStore()
        store.getProxy().extraParams = {};
        store.getProxy().extraParams.campoNome = campoNome;
        store.getProxy().extraParams.campoGenero = campoGenero;
        store.getProxy().extraParams.campoPreco = campoPreco;
        store.removeAll()
        store.load()


        // debugge;

    },
    onPesquisarClickFun: function (btn, e, eOpts) {
        var pesquisarFuncionario = Ext.ComponentQuery.query('[itemId=pesquisarFuncionario]')[0].getValue();
        var grid = btn.up('grid')
        var store = grid.getStore();
        store.getProxy().extraParams = {};
        store.getProxy().extraParams.pesquisarFuncionario = pesquisarFuncionario;
        store.removeAll();
        store.load();

    },
    onPesquisarClickCli: function (btn, e, eOpts) {
        var campo1Pesquisa = Ext.ComponentQuery.query('[itemId=campo1Pesquisa]')[0].getValue();
        var grid = btn.up('grid')
        var store = grid.getStore()
        store.getProxy().extraParams = {};
        store.getProxy().extraParams.campo1Pesquisa = campo1Pesquisa;
        store.removeAll();
        store.load()

    },

    //EXportar
    onExportClickCli: function (btn, e, eOpts) {
        window.location.href = 'php/PHPExcel/testeBruna/exceldocliente.php'

    },
    onExportTelaCli: function (btn, e, eOpts) {
        var campo1 = Ext.ComponentQuery.query('[itemId=campo1Pesquisa]')[0].getValue();
       
        var win = Ext.create('Ext.window.Window', {
            title: 'Baixando arquivo Excel',
            width: 300,
            height: 200,
            modal: true,
            layout: 'fit',
            maximizable: true
        });
        href = 'php/PHPExcel/testeBruna/exceltelacli.php?&campo1='+campo1;
        win.html = '<iframe src="' + href + '" name="principal" width="100%" height="100%" scrolling="auto" frameborder="0"></iframe>';
        win.show();
 //Manuntenção
 
    },

    // IMPORTAÇÂO
    onViewImportClickCli: function (btn, e, e0pts) {
        var win = Ext.create('ExtMVC.view.ImportCliente');



    },

    onImportClickCli: function (btn, e, eOpts) {
        var form = btn.up('form').getForm();
        console.log(form)
        if (form.isValid()) {
            form.submit({  //Botei no name o nome do parametro que foi passado para o back-end
                url: 'php/phpClienteVip/import/processa.php',
                waitMsg: 'Uploading',
                success: function (fp, o) {
                    msg('Success', 'Processed file "' + o.result.file + '" on the server');
                }
            });
        }
    },

    onCancelarImportCli: function (btn, e, e0pts) {
        var win = btn.up('window');
        var form = win.down('form');
        form.getForm().reset();
        win.close();

    }


})
