# HNU数据库大作业
>使用Swing写的图形化界面，MySQL作为数据库，由于是个人三天速通写完，实用可能算不上，但是应付下大作业感觉没啥问题。Swing导入了一个的flatlaf的美化包，可自行上网下载找到，如果懒得下载可以直接删掉导入的那部分代码也是你能正常运行，最后的实现结果贴在了ImgOfFrame文件夹下面。
## 教师
姓名；教师编号；密码

## 学生
姓名；学号；密码

## 课程
课程号；课程名；学分；选课人数；教师编号

## 选课表
课程号；学号；成绩；教师编号

## 关系
学生可以选课和查看课程表。
教师能修改选课表的成绩和在课程表中新增课程或者删除课程。
每门课程学分不能超过4分以上，选课人数不能超120人。
一名教师可以负责多门课程，但一门课程只能有一名教师负责。

## 相关sql语句
```
create database SCDB;
create table Student
(
    Sno   char(20) primary key,
    Sname char(20) not null,
    Spwd  char(20) not null
);

create table Teacher
(
    Tno   char(20) primary key,
    Tname char(20) not null,
    Tpwd  char(20) not null
);

create table Course
(
    Cno     char(4) primary key,
    Cname   char(20) not null,
    Ccredit smallint not null,
    Cnum    smallint,
    Tno     char(20) references Teacher (Tno),
    check ( 0 <= Ccredit <= 4 )
);

create table SC
(
    Cno   char(4) references Course (Cno) on delete cascade,
    Sno   char(20) references Student (Sno),
    Tno   char(20) references Teacher (Tno),
    Grade smallint,
    primary key (Cno, Sno),
    check ( 0 <= Grade <= 100 )
);

create user 'student'@'localhost' identified by 's123456';
create user 'teacher'@'localhost' identified by 't123456';

grant select on table Course to 'student'@'localhost';
grant select,insert on table SC to 'student'@'localhost';
grant select, insert, delete on table Course to 'teacher'@'localhost';
grant select, update on table SC to 'teacher'@'localhost';

create trigger check_course_num
    before insert
    on sc
    for each row
begin
    declare temp smallint;
    declare message varchar(20);
    select cnum into temp from course where Course.Cno = NEW.Cno;
    if (temp >= 120)
    then
        select XXX into message;
    else
        update course set cnum=cnum + 1 where Cno = NEW.Cno;
    end if;
end;
```
