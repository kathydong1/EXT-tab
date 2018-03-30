 Ext.define('Product', {     
        extend: 'Ext.data.Model',  
        fields: [ 'id', 'productname', 'desc', 'price' ]   
    });  
    var productStore = Ext.create('Ext.data.Store', {  
    model: 'Product',
    //为了对列进行分组，你需要在 store 的 groupField 属性上指定分组字段
    groupField: '董颖',
    //设置每一个10跳 
    // pageSize: 2,
    // autoLoad: true, 
    // proxy: {   
    //     发送ajax    
    //     type: 'ajax',   
    //     重后端取数据    
    //     url: 'data.json',       
    //     reader: {         
    //         type: 'json', 
    //         rootProperty 属性告诉 store 从 JSON 文件中哪个地方查找记录 
    //         rootProperty: 'data',
    //         totalProperty 属性让 store 知道从哪里读取记录总数         
    //         totalProperty: 'total'  
    //     }  
    // },    
    data: [{       
        id: 'P1',  
        productname: 'Ice Pop Maker',  
        desc: 'Create fun and healthy treats anytime',  
        price: '$16.33'  
    }, {       
        id: 'P2',  
        productname: 'Stainless Steel Food Jar',       
        desc: 'Thermos double wall vacuum insulated food jar',  
        price: '$14.87'  
    },{       
        id: 'P3',  
        productname: 'Shower Caddy',  
        desc: 'Non-slip grip keeps your caddy in place',  
        price: '$17.99'  
    }, {       
        id: 'P4',  
        productname: 'VoIP Phone Adapter',  
        desc: 'Works with Up to Four VoIP Services Across One Phone Port',  
        price: '$47.50'  
    }]   
});  
Ext.application({     
    name : 'Fiddle',     
    launch : function() {  
        //创建表格
        Ext.create('Ext.grid.Panel', {     
            renderTo: Ext.getBody(),     
            store: productStore,  
            //引入排序插件gridfilters
            //引入编辑插件cellediting对整列进行编辑   Ext.grid.plugin.CellEditing
            //引入编辑插件rowediting对整行进行编辑    Ext.grid.plugin.RowEditing
            plugins: ['cellediting','gridfilters'], 
            width: 600,     
            title: 'Products',
            //对列进行分组 Ext.grid.feature.Feature   
            features: [{       
                id: 'group',       
                ftype: 'grouping',
                //分组后你想要的信息       
                groupHeaderTpl : '{columnName}',    
                hideGroupedHeader: true, 
                //用来关闭在运行时通过 grid 的其他字段分组的默认属性    
                enableGroupingMenu: false  
            }],    
            columns: [{       
                text: 'Id',       
                dataIndex: 'id',  
                //控制显示隐藏     
                hidden: true  
            },{       
                text: 'Name',  
                //指定当前表格宽度     
                width:150,  
                dataIndex: 'productname',
                filter:'string', 
                //需要先plugin进来排序插件，然后对当前列列进行输入框编辑
                // editor: {         
                //     allowBlank: false,         
                //     type: 'string'  
                // } 
                //可以赛选的编辑
                editor: new Ext.form.field.ComboBox({  
                    typeAhead: true,         
                    triggerAction: 'all', 
                    //一旦编辑了记录，默认不会存到服务器。你可以设置 store 的 autosync 属性为 true
                    //你不希望同步立即发生，那么你可以在需要时调用 store 的 save 或 sync 方法 
                    store: [  
                        [ 'Bath','Bath'],  
                        [ 'Kitchen','Kitchen'],  
                        [ 'Electronic','Electronic'],  
                        [ 'Food', 'Food'],  
                        [ 'Computer', 'Computer' ]  
                    ]  
           
                })    
            },{  
                //文本描述
                text: 'Description', 
                //指定对应 Product 模型中的哪个字段      
                dataIndex: 'desc',  
                //是否允许排序,默认的排序是客户端排序。想开启服务端排序,需要为 store 设置 remoteSort 属性 为 true   
                sortable: false,    
                //使用剩余宽度   
                flex: 1,
                //需要先plugin进来排序插件，然后对列排序
                filter: {
                    //对每一列你都可以指定过滤类型         
                    type: 'string',  
                    itemDefaults: { 
                        // emptyText 就是空文本时的提示内容    
                        emptyText: 'Search for…'   
                    }  
                }  
                
            },{       
                text: 'price',       
                width: 100,       
                dataIndex: 'price',
                // 改变列renderer 配置可以用来改变列内容的呈现方式
                renderer: function(value) {  
                    return Ext.String.format('${0}', value);  
           
                }  
            },
        
        ],
            //dockedItems是Ext.panel.Panel的一个属性
            dockedItems: [{  
                //分页工具栏 Ext.toolbar.Paging (xtype: pagingtoolbar)     
                xtype: 'pagingtoolbar',       
                store: productStore,
                //分页位置可以为上下左右       
                dock: 'bottom',       
                displayInfo: true  
            }]   
        }); 
