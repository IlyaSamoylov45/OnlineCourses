Trigger example 1 : SELF-TRIGGERING
```SQL
create trigger R1
after insert on T1
for each row
begin
  insert into T1 values (New.A+1);
end;

pragma recursive_triggers = on;

create trigger R1
after insert on T1
for each row
when (select count(*) from T1) < 10
begin
  insert into T1 values (New.A+1);
end;
```

Trigger example 2 : CYCLES
```SQL
create trigger R1
after insert on T1
for each row
begin
  insert into T2 values (New.A+1);
end;

create trigger R2
after insert on T2
for each row
begin
  insert into T3 values (New.A+1);
end;

create trigger R3
after insert on T3
for each row
when (select count(*) from T1) < 100
begin
  insert into T1 values (New.A+1);
end;

pragma recursive_triggers = on;
```

Trigger example 3 : CONFLICTING TRIGGERS
```SQL
create trigger R2
after insert on T1
for each row
when exists (select * from T1 where A = 2)
begin
  update T1 set A = 3;
end;

pragma recursive_triggers = on;

create trigger R1
after insert on T1
for each row
begin
  update T1 set A = 2;
end;
```

Trigger example 4 :  NESTED INVOCATIONS
```SQL
create trigger R1
after insert on T1
for each row
begin
  insert into T2 values (1);
  insert into T3 values (1);
end;

create trigger R2
after insert on T2
for each row
begin
  insert into T3 values (2);
  insert into T4 values (2);
end;

create trigger R3
after insert on T3
for each row
begin
  insert into T4 values (3);
end;
```

Trigger example 5 : ROW-LEVEL IMMEDIATE ACTIVATION
```SQL
create trigger R1
after insert on T1
for each row
begin
  insert into T2 select avg(A) from T1;
end;
```
