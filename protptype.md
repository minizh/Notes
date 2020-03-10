                prototype                                                                   					prototype   
            ——————————————>                            _proto_                            		——————————————> 
Person()                               Person.prototype   ——————————————>   Object.prototype                        		Object()
构造函数     <——————————————    实例原型                                            			<——————————————
   |            constructor               ↑                         |               							constructor
   |                                      |                         |
   |                                      |                         |  _proto_
   ↓                                      |                         ↓ 
 person    ———————————————————                         null
 实例对象            _proto_
