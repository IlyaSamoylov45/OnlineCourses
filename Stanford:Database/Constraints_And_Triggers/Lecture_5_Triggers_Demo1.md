Trigger example 1 : New students with GPA between 3.3 and 3.6 automatically apply to geology major at Stanford and biology at MIT

```SQL
create trigger R1
after insert on Student
for each row
when New.GPA > 3.3 and New.GPA <= 3.6
begin
  insert into Apply values (New.sID, 'Stanford', 'geology', null);
  insert into Apply values (New.sID, 'MIT', 'biology', null);
end;
```

Trigger example 2 : Cascaded delete for Apply(sID) references Student(sID)
```SQL
create trigger R2
after delete on Student
for each row
begin
  delete from Apply where sID = Old.sID;
end;
```

Trigger example 3 : Cascaded update for Apply(cName) references College(cName)
```SQL
create trigger R3
after update of cName on College
for each row
begin
  update Apply
  set cName = New.cName
  where cName = Old.cName;
end;
```

Trigger example 4 : Enforce key constraint on College.cName
```SQL
create trigger R4
before insert on College
for each row
when exists (select * from College where cName = New.cName)
begin
  select raise(ignore);
end;
```

Trigger example 5 : Enforce key constraint on College.cName
```SQL
create trigger R5
before update of cName on College
for each row
when exists (select * from College where cName = New.cName)
begin
  select raise(ignore);
end;
```

Trigger example 6 : When number of applicants exceeds 10, label College as 'Done'
```SQL
create trigger R6
after insert on Apply
for each row
when (select count(*) from Apply where cName = New.cName) > 10
begin
  update College set cName = cName || '-Done'
  where cName = New.cName;
end;
```

Trigger example 7 : Cannot insert student with sizeHS < 100 or sizeHS > 5000
```SQL
create trigger R7
before insert on Student
for each row
when New.sizeHS < 100 or New.sizeHS > 5000
begin
  select raise(ignore);
end;

create trigger R7
after insert on Student
for each row
when New.sizeHS < 100 or New.sizeHS > 5000
begin
  delete from Student where sID = New.sID;
end;
```

Trigger example 8 : Automatically accept to Berkeley students with high GPAs from large high schools
```SQL
create trigger AutoAccept
after insert on Apply
for each row
when (New.cName = 'Berkeley' and
      3.7 < (select GPA from Student where sID = New.sID) and
      1200 < (select sizeHS from Student where sID = New.sID))
begin
  update Apply
  set decision = 'Y'
  where sID = New.sID
  and cName = New.cName;
end;
```

Trigger example 9 : When enrollment goes from below 16000 to above 16000, remove 'EE' applications and set all accepts to 'U' (undecided)
```SQL
create trigger TooMany
after update of enrollment on College
for each row
when (Old.enrollment <= 16000 and New.enrollment > 16000)
begin
  delete from Apply
    where cName = New.cName and major = 'EE';
  update Apply
    set decision = 'U'
    where cName = New.cName
    and decision = 'Y';
end;
```
