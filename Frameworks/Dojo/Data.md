#Creating Stores
基于Indexed Database
##dojo/store/Memory

	//data could be a JSON Array	
	var store=new Memory({data：data})
	//return obj	
	store.get(id)
	//return Array
	store.query({key:avlue})
	//return sorted Array
	store.query({key:value},{sort:[{attribute: key}]/function})
	//add
	store.put(obj)
	//remove
	store.remove(id)

##Creating store
###Internal data structures
idProperty:识别标记，默认为id
###query
query(queryobj,opt)
####query engine

	require(["dojo/_base/array"],
	        function(arrayUtil){
	var myEngine = function(query, options){
	    var filteringFunction = function(object){
	        //    do something here based on the passed query object
	    };
	
	    var execute = function(array){
	        var results = arrayUtil.filter(array, filteringFunction);
	        //    do anything else needed, like sorting and pagination
	        return results;
	    }
	    execute.matches = filteringFunction;
	    return execute;
	}

####QueryResults
使用QueryResults包裹query engine返回的结果

	QueryResults(Array/Promise)

	query: function(query, options){
		//为query添加迭代方法
	    return QueryResults(
	        (this.queryEngine(query, options))(this.data)
	    );
	}

 - query
	 - queryEngine
	 - QueryResults
 - get
 - getIdentity
 - put
 - add
 - remove

		define(["dojo/store/util/QueryResults", "dojo/_base/declare", "dojo/store/util/SimpleQueryEngine"],
		        function(QueryResults, declare, SimpleQueryEngine){
		
		    //    Declare the initial store
		    return declare(null, {
		        data: [],
		        index: {},
		        idProperty: "id",
		        queryEngine: SimpleQueryEngine,
		
		        constructor: function(options){
		            lang.mixin(this, options || {});
		            this.setData(this.data || []);
		        },
		        get: function(id){
		            return this.index[id];
		        },
		        getIdentity: function(object){
		            return object[this.idProperty];
		        },
		        put: function(object, options){
		            var id = options && options.id
		                || object[this.idProperty];
		            this.index[id] = object;
		
		            var data = this.data,
		                idProperty = this.idProperty;
		            for(var i = 0, l = data.length; i < l; i++){
		                if(data[i][idProperty] == id){
		                    data[i] = object;
		                    return id;
		                }
		            }
		            this.data.push(object);
		            return id;
		        },
		        add: function(object, options){
		            var id = options && options.id
		                || object[this.idProperty];
		            if(this.index[id]){
		                throw new Error("Object already exists");
		            }
		            return this.put(object, options);
		        },
		        remove: function(id){
		            delete this.index[id];
		            for(var i = 0, l = this.data.length; i < l; i++){
		                if(this.data[i][this.idProperty] == id){
		                    this.data.splice(i, 1);
		                    return;
		                }
		            }
		        },
		        query: function(query, options){
		            return QueryResults(
		                (this.queryEngine(query, options))(this.data)
		            );
		        },
		        setData: function(data){
		            this.data = data;
		            //    index our data
		            this.index = {};
		            for(var i = 0, l = data.length; i < l; i++){
		                var object = data[i];
		                this.index[object[this.idProperty]] = object;
		            }
		        }
		    });
		});

---

#MVC
##dojo/Stateful
继承dojo/Stateful，实现watch：

	var handle = myObj.watch("foo", function(name, oldValue, value){
	  console.log(name, oldValue, value);
	});
	handle.unwatch();

 - get()
 - set()
 - watch()
 - _changeAttrValue()

###Custom Accessors

 - _xxxGetter
 - _xxxSetter

在declare中，私有属性需在原型中设为null，并在constructor中实例化：
如

	var Person = declare({
		//原型
		firstName: null,
		lastName: null,
		company: null,
		getFullName: function() {
			return this.firstName+ " " + this.lastName;
		},
	  // ...
	  	constructor:function(firstName,lastName,company){
			this.xx=xx
		}
	});

当继承stateful后，无需再调用构造函数，直接传入一个键值对对象作为参数即可

	var Person = declare([Stateful], {
		firstName: null,
		lastName: null,
		company: null,
		getFullName: function() {
		return this.get("firstName") + " " + this.get("lastName");
		},
		// ...
	});

若空对象初始值不设为null或undefined，则需要构造函数
##Model

---

#Datagrid
##Cells

	store = new Memory({ data: data.items });
	dataStore = new ObjectStore({ objectStore: store });
	var dataGrid=new DataGrid({
        store:dataStore,
        structure:[
            {name:"NAME",field:"name",width:"100px"},
            {name:"AGE",field:"age",width:"200px"},
        ]
    },"grid")

 - structure：obj
	 - name: 表头名称
	 - field: data中对应的key
	 - width
	 - hidden: true则隐藏该列
	 - class(class钩在对应的container上)
		 - headerClasses(表头)
		 - cellClasses（表格）
		 - classes（所有）
	 - style
		 - headerStyles
		 - cellStyles
		 - styles

##Sub-Row(子列)

 - table-layout: fixed(宽度由布局决定，加速渲染)
 - 第一个子列不能跨列
 - structure:[]

		grid = new DataGrid({
		    store: store,
		    query: { id: "*" },
		    structure: [
		        [
		            { name: "First Name", field: "first", width: "84px", rowSpan: 2 },
		            { name: "Last Name", field: "last", width: "84px", rowSpan: 2 },
		            { name: "BB", field: "totalBB", width: "60px" },
		            { name: "K", field: "totalK", width: "60px" }
		        ],[
		            { name: "Games as Batter", field: "totalGAB", colSpan: 2 },
		            { name: "H", field: "totalH" },
		        ]
		    ]
		}, "grid");
	
##Views
锁定列

	structure: [
        {
			//锁定两列
            noscroll: true,
			//设置默认宽度，cells无需再重复指定，默认6em
			defaultCell: { width: "84px" },
            cells: [
                { name: "First Name", field: "first"},
                { name: "Last Name", field: "last" }
            ]
        },{
            cells: [
                [
                    { name: "Bats", field: "bats", width: "70px", rowSpan: 2 },
                    { name: "Throws", field: "throws", width: "70px", rowSpan: 2 },
                ],[
                    { name: "Games as Batter", field: "totalGAB", colSpan: 2 },
                    { name: "H", field: "totalH" },
                ]
            ]
        }
    ]

##Paging and Virtual Scrolling(分页和虚拟滚动)

 - keepRows
 - rowsPerPage

---

#Dojo Object Store
##dojo/store/Memory

 - 基础store
 - 同步store
 - 对store的操作会影响到data
 - 不要直接对data进行操作，会引起索引混乱，使用get+put的方式修改

		var data;
		var memory=new Memory({data:data,idProperty: key})
		memory.query({key:value})
		memory.add({})(不允许重复)
		memory.remove(<idProperty>)
		memory.put()（修改）

###query
query:

 - @params1:{key:value}
 - @params2:{} 配合分页器使用
	 - sort：[{attribute:"department", descending: false}]
	 - start
	 - count

##dojo/store/JsonRest
HTTP/REST 


{"msgType":"item","createdStart":"2015-12-01 00:00:00","created":"2015-12-24 16:28:15"}




	
