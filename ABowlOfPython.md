#San Diego CTF
## A Bowl of pythons

Category | Points 
--- | --- 
Revenge| 129

![screen](public/pythons_0.png)

- In this task, we are given a python script, makes a flag inside and then compares it with an input
- This is an initial code
```
FLAG = 'sdctf{a_v3ry_s3cur3_w4y_t0_st0r3_ur_FLAG}' # lol

a = lambda n: a(n-2) + a(n-1) if n >= 2 else (2 if n == 0 else 1)

b = lambda x: bytes.fromhex(x).decode()

h = eval(b('7072696e74'))

def d():
    h(b('496e636f727265637420666c61672120596f75206e65656420746f206861636b206465657065722e2e2e'))
    eval(b('5f5f696d706f72745f5f282273797322292e65786974283129'))
    h(FLAG)

def e(f):
    h("Welcome to SDCTF's the first Reverse Engineering challenge.")
    c = input("Input the correct flag: ")
    if c[:6].encode().hex() != '{2}3{0}{1}{0}3{2}{1}{0}{0}{2}b'.format(*map(str, [6, 4, 7])):
        d()
    if c[int(chr(45) + chr(49))] != chr(125):
        d()
    g = c[6:-1].encode()
    if bytes( (g[i] ^ (a(i) & 0xff) for i in range(len(g))) ) != f:
        d()
    h(b('4e696365206a6f622e20596f7520676f742074686520636f727265637420666c616721'))

if __name__ == "__main__":
    e(b't2q}*\x7f&n[5V\xb42a\x7f3\xac\x87\xe6\xb4')
else:
    eval(b('5f5f696d706f72745f5f282273797322292e65786974283029'))
```
- First of all this is not the flag and it's a pity
```
FLAG = 'sdctf{a_v3ry_s3cur3_w4y_t0_st0r3_ur_FLAG}' # lol
```
- Lets deobfuscate the code
```
FLAG = 'sdctf{a_v3ry_s3cur3_w4y_t0_st0r3_ur_FLAG}' # lol

a = lambda n: a(n-2) + a(n-1) if n >= 2 else (2 if n == 0 else 1)

b = lambda x: bytes.fromhex(x).decode()

#print(bytes.fromhex('7072696e74').decode('utf-8')) -> 'print'
h = print

def d():
    # print(bytes.fromhex('496e636f727265637420666c61672120596f75206e65656420746f206861636b206465657065722e2e2e').decode('utf-8'))
    print('Incorrect flag! You need to hack deeper...')
    #print(bytes.fromhex('5f5f696d706f72745f5f282273797322292e65786974283129').decode('utf-8'))
    __import__("sys").exit(1)
    h(FLAG)

def e(f):
    print("Welcome to SDCTF's the first Reverse Engineering challenge.")
    c = input("Input the correct flag: ")
    #print(bytes.fromhex('{2}3{0}{1}{0}3{2}{1}{0}{0}{2}b'.format(*map(str, [6, 4, 7]))).decode('utf-8'))
    if c[:6].encode().hex() != 'sdctf{':
        d()
    if c[:-1] != '}':
        d()
    g = c[6:-1].encode()
    if bytes( (g[i] ^ (a(i) & 0xff) for i in range(len(g))) ) != f:
        d()
    # print(bytes.fromhex('4e696365206a6f622e20596f7520676f742074686520636f727265637420666c616721').decode('utf-8'))
    print('Nice job. You got the correct flag!')

if __name__ == "__main__":
    e(b't2q}*\x7f&n[5V\xb42a\x7f3\xac\x87\xe6\xb4')
else:
    __import__("sys").exit(0)
```
- Okey now we understand the code and already have first part of our flag, and the last one obviously will `}`

- So we should find out how middle part was encoded by the code above
```
a = lambda n: a(n-2) + a(n-1) if n >= 2 else (2 if n == 0 else 1)
g = c[6:-1].encode()
if bytes( (g[i] ^ (a(i) & 0xff) for i in range(len(g))) ) != f:
```
- `a` it's a i-th number in Fib sequence and ` & 0xff` it's a xor by module of `256`
we take the 7 least significant bits, and nullify the rest

- So, to decode tha middle part of the flag we should replace `g` and `f` from the args of the `e` function

```
f = b't2q}*\x7f&n[5V\xb42a\x7f3\xac\x87\xe6\xb4'
print(bytes( (f[i] ^ (a(i) & 0xff) for i in range(len(f))) ))
```

- Run the script and get the flag

flag=sdctf{v3ry-t4sty-sph4g3tt1}