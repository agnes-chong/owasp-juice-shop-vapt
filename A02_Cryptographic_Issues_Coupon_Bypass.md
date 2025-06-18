# 🔐 A02:2021 - Cryptographic Issues (Forged Coupon)
**Author:** Agnes Chong  
CTF/Assessment: OWASP Juice Shop VAPT (TAR UMT, BMIS3123)  
**Category:** Web | Cryptography  
**Severity:** 🔥 High  
**Date:** April 2025

---

## 📖 Description

This vulnerability involves **predictable coupon encoding** using **Z85**, a reversible encoding scheme. Because the coupon code logic lacked cryptographic signing and validation, I was able to **generate unauthorized discount coupons (up to 90%)**.

---

## 🔍 Observations

1. Coupons were shared on OWASP Juice Shop’s **Reddit page**.
2. I identified the format using the Z85 encoding scheme.
3. Each coupon decoded into a plaintext format like:  
   `APR25-40` → `Month-Year-Discount`
4. System didn’t verify coupon origin or digital signatures.

---

## 🛠️ Tools Used

- **Python** with custom Z85 encode/decode script
- OWASP Juice Shop (v17.2.0) — running locally
- Browser dev tools

---

## 🧪 Exploitation Steps

### 🔹 1. Found Public Coupon
- `k#pDmh7ZTs` posted on Reddit
- 10-char Z85 format

### 🔹 2. Decoded Coupon (Python)

```python
from base64 import b85decode
code = 'k#pDmh7ZTs'
decoded = b85decode(code.encode())
print(decoded.decode())
# Output: APR25-40

### 🔹 3. Modified to 90% Discount

```python
from base64 import b85encode
new_coupon = 'APR25-90'
encoded = b85encode(new_coupon.encode())
print(encoded.decode())
# Output: k#pDmh7Z*x

### 🔹 4. Used in Juice Shop Checkout
Applied code k#pDmh7Z*x
✅ 90% discount applied instantly

