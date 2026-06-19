# KPL Compiler — Code Generation (Lab 4, Part 2)

Bài thực hành môn *Experiment in Compiler Construction* (HUST) — sinh mã đích (stack machine) cho bộ biên dịch KPL.

## Đã hoàn thiện

- `src/codegen.c`: `genVariableAddress`, `genVariableValue` — tính `(level, offset)` bằng cách đi ngược chuỗi scope, xử lý đúng tham số truyền theo tham chiếu (`PARAM_REFERENCE`); dùng `LV` trực tiếp khi đọc giá trị biến thường (không sinh dư `LA+LI`).
- `src/parser.c`: sinh mã cho l-value, phép gán, `if`/`if-else`, `while`, `for` (đúng pattern giữ địa chỉ biến chạy trên stack suốt vòng lặp), biểu thức điều kiện (so sánh), biểu thức số học (`+ - * /`, dấu trừ một ngôi).

## Build

```
cd src
make
```

## Test (byte-exact với bytecode tham chiếu)

`tests/example{1,2,4}.txt` là bytecode đã compile sẵn (giống byte-từng-byte với file `example{1,2,4}` không đuôi trong cùng thư mục — đây là format file `Instruction` ghi thẳng, không header, theo `saveCode`/`loadCode` trong `instructions.c`). So khớp bằng `cmp` là cách test chính xác nhất, chặt hơn đọc `-dump` bằng mắt:

```
cd src
./kplc ../tests/example1.kpl /tmp/out1.o
./kplc ../tests/example2.kpl /tmp/out2.o
./kplc ../tests/example4.kpl /tmp/out4.o

cmp /tmp/out1.o ../tests/example1.txt   # không in gì = khớp 100%
cmp /tmp/out2.o ../tests/example2.txt
cmp /tmp/out4.o ../tests/example4.txt
```

Hiện tại cả 3 file đều khớp byte-exact với bản tham chiếu.
