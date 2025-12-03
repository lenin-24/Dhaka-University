## Assigning int ,string and float 


```python
x = 9
print(x)
print(type(x))

x = "abc"
print(x)
print(type(x))

x = 45.12
print(x)
print(type(x))
```

## Assigning multiple variables in same line
```python
x, y, z = 9, 5, "abc"
```

## python data types


### bool
```python
x = True
```
### list
```
x = [1,2,3,4,5.7, "abc"]
print(x)
print(x[4])
x[2] = 11
print(x)
x.append(2)
print(x)
print(len(x))
print(type(x))
```

### tuple
```
x = (1,2, "abc")
print(x)
print(x[0])
```
### dict
```
x  = {"bob": 10, "alice": 11, "john": 12}
print(x)
print(x["john"])
print(type(x))
```
## arithmetic operations
```
z = x % y # modulus
z = x // y # floor
z = x ** y # exponentiation
z = x/(y + y) - z # multiple operations
```
## casting
```
s = "123"
print(s)
print(type(s))
x = 1
print(x)
print(type(x))
y = int(s) # str to int
print(y)
print(type(y))
z = x + y
print(z)

x = 11234.2345345
print(type(x))
y = int(x) # float to int
print(y)
print(type(y))

s = "123.455"
print(type(s))
x = float(s) # str to float
print(x)
print(type(x))
print(x +4)

s = "3"
x = float(s) # str to float
print(x)
print(type(x))

y = 3
x = float(y) # int to float
print(x)
print(type(x))

y = 3
s = str(y) # int to str
print(s)
print(type(s))
```
