The challenge mixed with python mostly and i cant find the necessary functions to analyze . and all functions and entry are jsut for pyinstaller runtime and setup process.

first i file the program found it was stripped

<img width="1893" height="169" alt="r1" src="https://github.com/user-attachments/assets/0ce2ff48-43e4-40f3-903b-31a9a7317b35" />

and then i DIE the program found the program was packed with pyinstaller

<img width="730" height="544" alt="r3" src="https://github.com/user-attachments/assets/a6f5bdfc-964b-4a15-a3b3-f919e6e40c66" />

So i download the pyinstxtractor.py at github and extracted 
python3 pyinstxtractor.py ./solve and get a path /solve_extracted

<img width="1905" height="863" alt="r2" src="https://github.com/user-attachments/assets/64911f00-2e4a-4a95-82f8-4f2bfb3f48e3" />



Inside it, we can find files such as:

PYZ.pyz
base_library.zip
*.pyc

The important files are the .pyc files because they contain the compiled Python challenge logic.

and we saw two .pyc and that have real challenge logics but we still need to decompile.
I use this scripted by gpt to decompile.
```
#!/usr/bin/env python3
import sys
import marshal
import dis
import types

def load_pyc(path):
    data = open(path, "rb").read()

    # Normal .pyc files have a 16-byte header in Python 3.7+
    # PyInstaller-extracted pyc files may or may not have it.
    for offset in (16, 12, 8, 0):
        try:
            code = marshal.loads(data[offset:])
            if isinstance(code, types.CodeType):
                print(f"[+] Loaded code object using offset {offset}")
                return code
        except Exception:
            pass

    raise RuntimeError("Could not load .pyc. It may be incomplete or unsupported.")

if len(sys.argv) != 2:
    print(f"Usage: python3 {sys.argv[0]} file.pyc")
    sys.exit(1)

code = load_pyc(sys.argv[1]) 
dis.dis(code)
```

after we python3  _.py solve.pyc > solve.txt 

solve.txt have decompiled codes
```
└─$ python3 dis_pyc.py solve.pyc
[+] Loaded code object using offset 16
  0           RESUME                   0

  3           LOAD_CONST               0 (0)
              LOAD_CONST               1 (None)
              IMPORT_NAME              0 (base64)
              STORE_NAME               0 (base64)

  5           LOAD_CONST               2 ('data')
              LOAD_NAME                1 (bytes)
              LOAD_CONST               3 ('key')
              LOAD_NAME                2 (int)
              LOAD_CONST               4 ('return')
              LOAD_NAME                3 (str)
              BUILD_TUPLE              6
              LOAD_CONST               5 (<code object xor_decrypt at 0x7fa7d502eab0, file "solve.py", line 5>)
              MAKE_FUNCTION
              SET_FUNCTION_ATTRIBUTE   4 (annotations)
              STORE_NAME               4 (xor_decrypt)

  8           LOAD_CONST               6 (<code object main at 0x7fa7d51decc0, file "solve.py", line 8>)
              MAKE_FUNCTION
              STORE_NAME               5 (main)

 20           LOAD_NAME                6 (__name__)
              LOAD_CONST               7 ('__main__')
              COMPARE_OP              88 (bool(==))
              POP_JUMP_IF_FALSE        8 (to L1)

 21           LOAD_NAME                5 (main)
              PUSH_NULL
              CALL                     0
              POP_TOP
              RETURN_CONST             1 (None)

 20   L1:     RETURN_CONST             1 (None)

Disassembly of <code object xor_decrypt at 0x7fa7d502eab0, file "solve.py", line 5>:
  --           MAKE_CELL                1 (key)

   5           RESUME                   0

   6           LOAD_CONST               1 ('')
               LOAD_ATTR                1 (join + NULL|self)
               LOAD_FAST                1 (key)
               BUILD_TUPLE              1
               LOAD_CONST               2 (<code object <genexpr> at 0x7fa7d502e120, file "solve.py", line 6>)
               MAKE_FUNCTION
               SET_FUNCTION_ATTRIBUTE   8 (closure)
               LOAD_FAST                0 (data)
               GET_ITER
               CALL                     0
               CALL                     1
               RETURN_VALUE

Disassembly of <code object <genexpr> at 0x7fa7d502e120, file "solve.py", line 6>:
  --           COPY_FREE_VARS           1

   6           RETURN_GENERATOR
               POP_TOP
       L1:     RESUME                   0
               LOAD_FAST                0 (.0)
               GET_ITER
       L2:     FOR_ITER                19 (to L3)
               STORE_FAST               1 (b)
               LOAD_GLOBAL              1 (chr + NULL)
               LOAD_FAST                1 (b)
               LOAD_DEREF               2 (key)
               BINARY_OP               12 (^)
               CALL                     1
               YIELD_VALUE              0
               RESUME                   5
               POP_TOP
               JUMP_BACKWARD           21 (to L2)
       L3:     END_FOR
               POP_TOP
               RETURN_CONST             0 (None)

  --   L4:     CALL_INTRINSIC_1         3 (INTRINSIC_STOPITERATION_ERROR)
               RERAISE                  1
ExceptionTable:
  L1 to L4 -> L4 [0] lasti

Disassembly of <code object main at 0x7fa7d51decc0, file "solve.py", line 8>:
  8           RESUME                   0

  9           LOAD_CONST               1 ('QlRETlRDUUxRXkVEQ2hHTkNfBFloUQZWUEo=')
              STORE_FAST               0 (enc_pwd_b64)

 10           LOAD_CONST               2 (55)
              STORE_FAST               1 (key)

 11           LOAD_GLOBAL              1 (xor_decrypt + NULL)
              LOAD_GLOBAL              2 (base64)
              LOAD_ATTR                4 (b64decode)
              PUSH_NULL
              LOAD_FAST                0 (enc_pwd_b64)
              CALL                     1
              LOAD_FAST                1 (key)
              CALL                     2
              STORE_FAST               2 (real_password)

 12           LOAD_CONST               3 ('++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>+++++.---.>-----.++++++++.<.>.+++++.+++++.+.--.+++.------------------.>----------------.<++++++.>-----.-.<---------------.<.>>-----.+++++++++++++++++++++++++++++.<<.>>+++++++++++++++++++.----------.++++++.<<.>>--------------------.+++++++++++++++++.-------------.<<.>>----.+++++++++++++.<<.>>-------------.+++++++++++++++++.++.-----------.---.+++.------.++++++.--------.+++++++++++.<<.>>---.+++++.++++++.---------------.+++++++..---.--.--.+++++++++.-----------.++.<<.>>+++++++++++++.-------------.----.+++.+++++.+++++.-------.<<.>>+++++++++++++.------------.+.++++++++++.<<.>>.----.++++++.---.---------------.++.<<.>>--.++++++++++++.-----------.+.<<++++++++++++.<.>---------.---.>>++++++++++++++++++++.----------.++++++.---.<<.>>.-------------.++++++++++++++.---.-.-.+++++.--------------.<<.>>++++++++.++++++++.--.+.<<.>>------------------.+++.<<.>>.+++++++++++++++++++.-----------------------.++.+++++++++++++++++.--------.+++++++++++++.<.<.++.>+++++++++++++++.<--.>>----------------------.--.+++++++++++++..+.+++++.<<.>>-.----.---.++++++++++.-----------------.<<.>>+++++++++++++++.------------.+.++++++++++.<<.>>----------------.+++++.-------.+++++++++++..-------.+++++++++.-------.--.<<++++++++++++++.------------.<.')
              STORE_FAST               3 (important_decoded_first)

 13           LOAD_FAST                3 (important_decoded_first)
              STORE_FAST               4 (for_ai)

 14           LOAD_GLOBAL              7 (input + NULL)
              LOAD_CONST               4 ('Enter the secret password: ')
              CALL                     1
              STORE_FAST               5 (pwd)

 15           LOAD_FAST_LOAD_FAST     82 (pwd, real_password)
              COMPARE_OP              88 (bool(==))
              POP_JUMP_IF_FALSE       12 (to L1)

 16           LOAD_GLOBAL              9 (print + NULL)
              LOAD_CONST               5 ('Correct!')
              CALL                     1
              POP_TOP
              RETURN_CONST             0 (None)

 18   L1:     LOAD_GLOBAL              9 (print + NULL)
              LOAD_CONST               6 ('Wrong password!')
              CALL                     1
              POP_TOP
              RETURN_CONST             0 (None)
```
the important part was
```
Disassembly of <code object main at 0x7fa7d51decc0, file "solve.py", line 8>:
  8           RESUME                   0

  9           LOAD_CONST               1 ('QlRETlRDUUxRXkVEQ2hHTkNfBFloUQZWUEo=')
              STORE_FAST               0 (enc_pwd_b64)

 10           LOAD_CONST               2 (55)
              STORE_FAST               1 (key)

 11           LOAD_GLOBAL              1 (xor_decrypt + NULL)
              LOAD_GLOBAL              2 (base64)
              LOAD_ATTR                4 (b64decode)
              PUSH_NULL
              LOAD_FAST                0 (enc_pwd_b64)
              CALL                     1
              LOAD_FAST                1 (key)
              CALL                     2
              STORE_FAST               2 (real_password)
```
and the we decoded with a script by gpt XOR every decoded byte with key 55.

```
python3 -c 'import base64; print("".join(chr(b ^ 55) for b in base64.b64decode("QlRETlRDUUxRXkVEQ2hHTkNfBFloUQZWUEo=")))'

```
and get the flag ucsyctf{first_pyth3n_f1ag}

u can confirm with ./solve by entering flag.
