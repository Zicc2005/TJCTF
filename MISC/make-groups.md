# 🕵️‍♂️ TJCTF 2024 – Forensics: `make-groups`

**Category:** Misc
![image](https://github.com/user-attachments/assets/47102bdc-b7bd-405e-9a82-1579bf1ef1d6)
Bài này nó bắt làm toán như c3 nên ta sẽ có 1 đoạn code tính toán, tuy nhiên code của bài sai nên ta sẽ chỉnh lại.
Đây là code mới:
```
f = [x.strip() for x in open("chall.txt").read().split('\n')]
n = int(f[0])
a = list(map(int, f[1].split()))
m = 998244353

# Precompute factorials and inverse factorials up to n
MAX = n + 1
fact = [1] * MAX
inv_fact = [1] * MAX

for i in range(1, MAX):
    fact[i] = fact[i-1] * i % m

# Fermat's Little Theorem: a^(m-2) ≡ a^(-1) mod m
inv_fact[MAX-1] = pow(fact[MAX-1], m-2, m)
for i in range(MAX-2, -1, -1):
    inv_fact[i] = inv_fact[i+1] * (i+1) % m

def choose(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % m * inv_fact[n-r] % m

ans = 1
for x in a:
    ans = ans * choose(n, x) % m

print(f"tjctf{{{ans}}}")
```

Hãy chạy code và ra falg:
![image](https://github.com/user-attachments/assets/56b35914-2ab1-427a-a600-95d139d8afc5)

Flag:tjctf{148042038}
