Exemplo de uma Store e um Model Extjs:
Ex: Store;

Ext.define('ExtMVC.store.Funcionarios', {
    extend: 'Ext.data.Store',
    model: 'ExtMVC.model.Funcionario', 
    autoLoad: true,
    pageSize: 35,
    autoLoad: {start: 0, limit: 35},
    
    proxy: { 
        type: 'ajax',
        api: {
        	create: 'php/criaFuncionario.php', 
            read: 'php/listaFuncionario.php',
            update: 'php/atualizaFuncionario.php',
            destroy: 'php/deletaFuncionario.php',
        },
        reader: {
            type: 'json',
            root: 'funcionarios', 
            successProperty: 'success'
        },
        writer: { 
            type: 'json',
            writeAllFields: true,
            encode: true,
            root: 'funcionarios',
        }
    }

    
});

Ex: de Model

Ext.define('ExtMVC.model.Funcionario',{
	extend: 'Ext.data.Model',
	fields: [ // campo
		{name: 'cod',  type: 'int'},
		{name: 'nome',  type: 'string'},
		{name: 'admissao',  type: 'string'},
		{name: 'situacao',  type: 'string'},
		{name: 'cargo',  type: 'string'}
	]
});
