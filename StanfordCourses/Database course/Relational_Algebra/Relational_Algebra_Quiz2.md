We've created a small sample database to use for this assignment. It contains four relations:

    Person(name, age, gender)       // name is a key
    Frequents(name, pizzeria)       // [name,pizzeria] is a key
    Eats(name, pizza)               // [name,pizza] is a key
    Serves(pizzeria, pizza, price)  // [pizzeria,pizza] is a key

1. Find all pizzas eaten by at least one female over the age of 20.
```
\project_{pizza}(
   \select_{gender='female' AND age>20} Person \join Eats
);
```

2. Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.)
```
\project_{name} (
  \select_{gender='female' AND pizzeria='Straw Hat'} (
    Person \join Eats \join Serves
  )
);
```

3. Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat.
```
\project_{pizzeria} (
    \select_{price<10} Serves
    \join
    \select_{name='Amy' OR name='Fay'} Eats
);
```

4. Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat.
```
\project_{pizzeria} (
    \select_{price<10} Serves
    \join
    (
        (\project_{pizza} \select_{name='Amy'} Eats)
        \intersect  
        (\project_{pizza} \select_{name='Fay'} Eats)
    )
);
```

5. Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos.
```
\project_{name} (
  Eats
  \join
  \project_{pizza} \select_{pizzeria='Dominos'} Serves
)
\diff (\project_{name} \select_{pizzeria='Dominos'} Frequents)
```

6. Find all pizzas that are eaten only by people younger than 24, or that cost less than $10 everywhere they're served.
```
\project_{pizza} Eats \diff (\project_{pizza}
(\select_{age>=24} Person \join Eats)
\intersect
\project_{pizza} (\select_{price>=10} Serves))

```

7. Find the age of the oldest person (or people) who eat mushroom pizza.
```
\project_{age}(
  (\select_{pizza = 'mushroom'}Eats)
  \join
  Person
)\project_{pizzeria} (Serves \join Person)
\diff
\project_{pizzeria} (
Serves
\join
((\project_{pizza} Serves)
\diff
(\project_{pizza}((\select_{age>'30'} Person) \join Eats)))
)
\diff
\project_{age1}(
  \select_{age1 < age2}(
    \rename_{age1}(\project_{age}((\select_{pizza = 'mushroom'}Eats) \join Person))
    \cross
    \rename_{age2}(\project_{age}((\select_{pizza = 'mushroom'}Eats) \join Person))
  )
)
```

8. Find all pizzerias that serve only pizzas eaten by people over 30.
```
\project_{pizzeria} (Serves \join Person)
\diff
\project_{pizzeria} (
  (Serves \join Person)
  \join
  (
    (\project_{pizza} Serves)
    \diff
    (\project_{pizza}((\select_{age>'30'} Person)
      \join Eats)
    )
  )  
)
```

9. Find all pizzerias that serve every pizza eaten by people over 30.
```
\project_{pizzeria}(Serves)
\diff
\project_{pizzeria}(
    (\project_{pizzeria}(Serves)
    \cross
    \project_{pizza}(\project_{name}(\select_{age>30}(Person))\join Eats))
    \diff
    \project_{pizzeria,pizza}(Serves)
)
```
  - Help from Stackoverflow
