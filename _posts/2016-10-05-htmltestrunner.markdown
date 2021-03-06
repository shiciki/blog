---
layout:     post
title:      "unittest搭建框架"
subtitle:   "用例整合以及HTMLTestRunner输出测试报告"
date:       2016-10-05 17:35:00
author:     "shiciki"
header-img: "img/post-bg-2015.jpg"
tags:
    - 自动化测试
    - python
    - unittest
    - HTMLTestRunner
---
### unittest

正如JAVA使用junit,python下也集成了一套测试框架unittest
官方有详细的文档，也分为[2.7](https://docs.python.org/2/library/unittest.html)和[3.0以上](https://docs.python.org/3/library/unittest.html)版本

首先可以使用testcase来实现单个用例的自动化，前提条件写入setup（）函数，setup会在每次执行testcase前执行，相对的，tearDown会在每次testcase执行后执行


	class homehelpTest(unittest.TestCase): 
     
	 
    def setUp(self):
        self.driver = webdriver.Remote('http://localhost:4723/wd/hub', devices())
        self.driver.wait_activity('.activity.ui.live.LiveActivity',10,1)
        self.number=3
        self.number=int(self.number)
	 
	 
    def test_case1(self):
        print(self.number)
        self.driver.find_element_by_name("xxx").click()
        a=(self.driver.is_ime_active())
        f=open('D:/workplace/test/result.txt','w+')
        f.write(str(a))
        f.close()
        self.assertEqual(a,True,msg=' keyboard is ok')
     
    def test_case2(self):          
        print(self.number)
        self.assertEqual(10,10,msg='keyboard is ok')
	 
    def tearDown(self):
        print('Test over')
        self.driver.quit()
	 
	if __name__=='__main__':
	 
		unittest.main()


编写case后可以使用suite来构造用例集，并使用addtest方法添加testcase
	suite = unittest.TestSuite()
	suite.addTest(homeHelpmefound.homehelpTest('test_case1'))
	
### HTMLTestRunner

HTMLTestRunner可以在[这里](http://tungwaiyip.info/software/HTMLTestRunner.html)下载
并且该地址还提供了一个demo  test_HTMLTestRunner.py 来作为参考

demo比较长，这里提供一个简单的demo作为参考

    runner = unittest.TextTestRunner()
    timestr = time.strftime('%Y%m%d%H%M%S',time.localtime(time.time()))
    filename = "E:\\eclipse\\autotest\\report\\demo_" + timestr + ".html"
    print (filename)
    fp = open(filename, 'wb')
    runner = mytool.HTMLTestRunner.HTMLTestRunner(
    stream = fp,
    title = '测试结果',
    description = '测试报告'
    )
    runner.run(suite)
    fp.close()
	
之后可以在设置的路径下生成简单测试报告
	
### python3下的HTMLTestRunner

HTMLTestRunner原作者是使用python2写的，所以使用python3的同学需要进行改写

[参考文章](http://bbs.chinaunix.net/thread-4154743-1-1.html)

第94行，将import StringIO修改成import io

第539行，将self.outputBuffer = StringIO.StringIO()修改成self.outputBuffer= io.StringIO()

第642行，将if not rmap.has_key(cls):修改成if notcls in rmap:

第766行，将uo = o.decode(‘latin-1‘)修改成uo = e

第775行，将ue = e.decode(‘latin-1‘)修改成ue = e

第631行，将print >> sys.stderr, ‘\nTime Elapsed: %s‘ %(self.stopTime-self.startTime)修改成print(sys.stderr, ‘\nTimeElapsed: %s‘ % (self.stopTime-self.startTime))

以上就是使用unittest和htmlrunner构建python自动化基础构架的基础流程。


