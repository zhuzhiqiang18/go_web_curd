# go_web_curd
go web 增删改查 封装了自己的orm

# ORM 
使用反射
# Model约定
1. tag sql 代表数据库中的字段 不注明 则使用结构体字段 
2. 数据库 不区分大小写
```go
type Student struct {
	Id int `sql:"id"` 
	Name string `sql:"name"`
	Address string `sql:"address"`
	No string
	ClassId int `sql:"class_id"`
}
```
# CURD使用方法
## 插入
```go
   db, err := orm.Open("root","123456","127.0.0.1",3306,"go_test")
   	defer db.Close()
   	if err!=nil {
   		fmt.Println(err)
   		return
   	}
   	var student model.Student
   	student.Name="张三"
   	student.Address="中国"
   	student.No="123456"
   	student.ClassId=1
   	student.Create = time.Now()
   	student.IsReading =true
   	res:= db.Save(&student)
   	fmt.Println("改变行数",res)
```
## 删除
```go
  db, _ := orm.Open("root","123456","127.0.0.1",3306,"go_test")
  	defer db.Close()
  	var student model.Student
  	student.ClassId=1
  	res:= db.Delete(&student,"class_id")
  	fmt.Println("改变行数",res)
```
## 更改
```go
db, _ := orm.Open("root","123456","127.0.0.1",3306,"go_test")
	defer db.Close()
	var student model.Student
	student.Name="张三"
	student.No="00000000"
	res:= db.Update(&student,"name")
	fmt.Println("改变行数",res)
```
## 查询
### 单表全查询
```go
db, _ := orm.Open("root","123456","127.0.0.1",3306,"go_test")
	defer db.Close()
	//传类型地址
	list := db.FindQuery(&model.Student{}, nil)
	for _,stu := range *list {
		fmt.Println(stu.(model.Student))
	}
```
### 单表指定字段查询
```go
db, _ := orm.Open("root","123456","127.0.0.1",3306,"go_test")
	defer db.Close()
	list := db.FindQuery(&model.Student{}, nil,"Name","No","Address")
	for _,stu := range *list {
		fmt.Println(stu.(model.Student).Name)
		fmt.Println(stu.(model.Student).No)
		fmt.Println(stu.(model.Student).Address)
	}
```
### 条件查询
>条件查询使用tag sql字段 


```go
db, _ := orm.Open("root","123456","127.0.0.1",3306,"go_test")
	defer db.Close()
	list := db.FindQuery(&model.Student{}, map[string]interface{}{"name": "张三"},"Name","No","Address")
	for _,stu := range *list {
		fmt.Println(stu.(model.Student).Name)
		fmt.Println(stu.(model.Student).No)
		fmt.Println(stu.(model.Student).Address)
	}
```
### 联合查询
待更新……
