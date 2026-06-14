# OOP

面向对象编程 (OOP) 是一种思想，一上来难以理解也是很正常的。

面向对象的核心总的来说是三条：封装、继承、多态

## 封装

<format color="YellowGreen">封装就是将事物抽象出来，化作笼统的概念</format>

要求：需要将具体事物共同拥有的属性、行为 (也叫做方法) 抽出来

学生都具有姓名、学号、成绩等属性，具有与学习相关的行为

```Python
class Student:
    def __init__(self,name:str,stu_num:int,scores:dict[str,int]):
        self.name = name
        self.stu_num = stu_num
        self.scores = scores

    def set_score(self,subject:str,score:int):
        """ 设置学生某科的成绩 """
        self.scores[subject] = score

    def get_score(self):
        """ 打印该学生的所有科目成绩 """
        print(self.scores)
```

## 继承

<format color="YellowGreen">将抽象化概念的范围作比较，小 (子类) 的继承大 (父类) 的。</format>

要求：父类需要更大范围的属性和行为 (子类共有的属性和行为)，非共有部分是子类特有的属性和行为

例如东北虎是老虎，但也归属猫科动物

而老虎分为华南虎、东北虎等不同种类

在这个角度上看就是华南虎是老虎的具体实例，继承自猫科动物这个大的种类

```Python
class Mammal:
    def __init__(self, name: str, sex: str, eye_num: int):
        self.name: str = name
        self.sex: str = sex
        self.eye_num: int = eye_num

    def breathe(self):
        print(f"{self.name}在呼吸")


class Human(Mammal):
    def __init__(self, name: str, sex: str, eye_num: int, has_tail: bool):
        super().__init__(name, sex, eye_num)
        self.has_tail: bool = has_tail

    def read(self):
        print(f"{self.name}在阅读")
```

## 多态

<format color="YellowGreen">同一行为在不同实例身上，表现出的多种不同的形式和结果</format>

要求：
- 必须是继承关系，并且是父类具有的而非子类特有的行为
- 叛逆的子类明修栈道暗度陈仓，修改了父类行为具体的实现形式 (重写行为)
- 子类实例被数据类型是父类的变量所保存（数据类型是父类的变量被赋值了子类的实例值）

多态是面向对象中最难理解的，他表现更加丰富，且具有上述要求的特点。

```Python
from typing import override


class Employee:
    def __init__(self, name: str, id: int) -> None:
        self.name = name
        self.id = id

    def print_info(self):
        print(f"姓名：{self.name},工号：{self.id}")

    def calculate_monthly_pay(self):
        print("月薪")


class FullTimeEmployee(Employee):
    def __init__(self, name: str, id: int, monthly_salary: float) -> None:
        super().__init__(name, id)
        self.monthly_salary = monthly_salary

    @override
    def calculate_monthly_pay(self):
        print(self.monthly_salary)


class PartTimeEmployee(Employee):
    def __init__(
        self, name: str, id: int, daily_salary: float, work_days: float
    ) -> None:
        super().__init__(name, id)

        self.daily_salary = daily_salary
        self.work_days = work_days

    @override
    def calculate_monthly_pay(self):
        salary = self.daily_salary * self.work_days
        print(salary)
```

Python 在 3.12 版本支持了 `@overrid` 装饰器。

如果不使用装饰器，即使写错了也不会报错，因为符合语法，但已经不属于重写父类方法了。

而 `@override` 的功能就是将不符合父类方法定义的重写报错提示，如果你的 IDE 支持的话。