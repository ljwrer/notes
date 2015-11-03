#Creating Builds
###Modules and Packages
package-->package.json

package.js  Dojo-specific build information
###dojoConfig
###Layers

	layers: {
		"dojo/myGrid": {
			include: [  
						"dojo/json","dojo/data/ObjectStore", 
	
                        "dojox/grid/enhanced/plugins/Pagination", 

                        "dojox/grid/enhanced/plugins/Filter", 

                        "dojox/grid/EnhancedGrid", 

                        "dojo/store/JsonRest", 

                        "dojo/domReady" ],
			customBase: true,
			boot: true
		}
	}

###Build Profiles
