#单元测试
测试出错误为目的

#函数测试

 - 函数是否被定义
 - 功能是否实现
 - 参数识别，额外测试结果

#定义期望

 - 预期正确结果
 - 预期错误结果
 - 不符预期
 - 修改函数
 - 重新测试
 - 红绿重构周期

		expect(add(0.1,0.2)).toBe(0.3);
		expect(add(2,'test')).toThrow();
		expect(add(0.1,0.2)).toBe(0.3);

#jasmine
[http://jasmine.github.io/2.2/introduction.html](http://jasmine.github.io/2.2/introduction.html)

 - source file---application files
 - spec file---test files

describe和it创建大纲，组织信息

###describe
定义测试集
###it

 - 定义测试边界
 - 验证spec
 - spec---测试用例，测试容器
	
		it("should be able to play a Song", function() {
		    player.play(song);
		    expect(player.currentlyPlayingSong).toEqual(song);
		    //demonstrates use of custom matcher
		    expect(player).toBePlaying(song);
		});

###expect
测试启动点

	expect（<acture>）.<negate>?.<matcher>(<expect>)
	//add(0.1,0.2)===0.3
	expect(add(0.1,0.2)).toBe.（0.3）
	expect(add(0.1,0.2)).not.toBe.（0.3）

#red-green-refactor cycle
先写测试单元

	describe("addressBook", function() {
		//通用数据
		var myAddressBook,
			thisContact;
		//测试前置
		beforeEach(function() {
			myAddressBook = new AddressBook();
			thisContact = new Contact();
		});
		//测试spec
		it("should be able to add contact", function() {
			myAddressBook.add(thisContact);
			expect(myAddressBook.getContact(0)).toBe(thisContact)
		});
		//测试spe
		it("should be able to delete contact", function() {
			myAddressBook.add(thisContact);
			myAddressBook.deleteContact(0);
			expect(myAddressBook.getContact(0)).not.toBeDefined();
		});
	})

###beforeEach
unit通用前置

###done
异步测试

	//异步方法
	getInitialContacts:function(cb){
		var self=this;
		setTimeout(function(){
			self.initialComplete=true;
			if(cb){
				cb();
			}
		},3)
	}

	//测试spec
	describe("Async address book", function() {
		var myAddressBook = new AddressBook();
		beforeEach(function(done) {
			myAddressBook.getInitialContacts(function() {
				done();
			})
		})
		it("should grap initial contacts", function(done) {
			expect(myAddressBook.initialComplete).toBe(true);
			done();
		})
	})

 1. beforeEach回掉done
 2. done执行完执行expect
 3. 最后回掉done结束expect