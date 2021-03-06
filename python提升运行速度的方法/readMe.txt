1.避免全局变量
	通过脚本语句将变量放到函数中，可以提升15%-30%的运行速度。
	例如：
		size = 10 （全局变量）
	应该改为：
		def Debug():
			size = 10 (局部变量)
2.避免模块和函数属性访问
	例如：math.sqrt()会拖慢运行速度，因为每次进行属性访问时都会调用__getattribute__和__getattr__()函数，可以通过from import语句消除
	如：from math import sqrt, 此时的sqrt还是为全局变量
	
	sqrt也可以写成局部变量来加快运行速度
	import math
	def Debug():
		sqrt = math.sqrt
		
	避免类内属性访问，如：self.x的访问速度会慢于x，
	如果要循环访问self.x，最好是将self.x的值赋给x，如果单次访问self.x，则无需赋值给局部变量x。
3.避免不必要的抽象
	尽量不要使用装饰器，属性访问器和描述器去处理对象，用简单的属性访问即可
4.1避免不必要的赋值，python中的copy()，deepcoy()通常是可以去掉的，这类函数尽量减少使用。
4.2交换时避免使用中间变量，例如要交换a, b的值。a, b = b, a
4.3字符串拼接用.join()而不是+
	.join()会一次性地计算出需要的所有空间，一次性申请，然后将每个元素复制到该内存中去
5.利用if条件地短路特性
	if A and B 若A为False直接返回结果
	if A or B，若A为True直接返回True
	所以书写时and中应该将错误性高的语句写在前面，or将正确性高的语句写在前面
6.循环优化
6.1.用for循环代替while循环
	for循环优于while循环，比while快不少
6.2使用隐式for循环代替显式for循环
	def computeSum(size: int) -> int:
		_sum = 0
		for i in range(size):
			_sum += i
		return _sum
	上述代码等同于：
	def computeSum(size: int) -> int:
		return sum(range(size))
	
6.3.减少内层for循环的计算量
	for x in range(5):
		for y in range(5):
			z = sqrt(x) + sqrt(y)
	应该改为：
	for x in range(5):
		sqrt_x = sqrt(x)
		for y in range(5):
			z = sqrt_x + sqrt(y)	
7.使用numba.jit
	numba.jit可以将python函数jit编译为机器码执行，大大提高代码运行速度，可以在如下页面找到更多的numba信息
	import numba
	
	
	#numba
	def compteSum(size: float) -> int:
		sum = 0
		for i in range(size):
			sum += 1
		return sum
		
	def main():
		size = 10000
		for _ in range(size):
		sum = comuteSum(size)
		
	
	main()
	
8.选择合适的数据结构
	list是动态的，当存储空间大于申请空间时会继续申请更大的空间，所以操作时会不断地申请释放空间，因此效率不高。
	因此应该考虑collection.deque双端队列来提高效率
	
	list的查找操作也很耗时，如果需要频繁查找某些元素，或频繁有序访问这些元素时，可以使用bisect维护list，并进行二分查找提高效率。
	
	查找最大值和最小值时，可以使用headpq模块将list转换为一个堆。(个人觉得numpy数组它不香吗？)
