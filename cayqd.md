# Phân tích và xây dựng cây quyết định để tìm nút gốc

## Dữ liệu ban đầu

Dữ liệu được cung cấp trong bảng sau:

| Mẫu | Hài   | Ngươi nổi tiếng | Theo trend | Có tranh luận | THÍCH |
|------|-------|-----------------|------------|---------------|-------|
| 1    | Không | Không           | Có         | Không         | Sai   |
| 2    | Có    | Không           | Có         | Không         | Đúng  |
| 3    | Không | Không           | Có         | Có            | Đúng  |
| 4    | Không | Có              | Có         | Không         | Đúng  |
| 5    | Không | Có              | Không      | Có            | Sai   |
| 6    | Có    | Có              | Có         | Không         | Sai   |

- **Tổng số mẫu**: 6.
- **THÍCH = Đúng**: 3 (mẫu 2, 3, 4).
- **THÍCH = Sai**: 3 (mẫu 1, 5, 6).
- Tỷ lệ p+ (THÍCH = Đúng) = 3/6 = 0.5.
- Tỷ lệ p- (THÍCH = Sai) = 3/6 = 0.5.

## Bước 1: Tính Entropy của tập dữ liệu ban đầu (S)

Công thức Entropy:  
Entropy(S) = -p+ * log2(p+) - p- * log2(p-)

Thay số:  
Entropy(S) = -0.5 * log2(0.5) - 0.5 * log2(0.5)  
= -0.5 * (-1) - 0.5 * (-1) = 0.5 + 0.5 = 1

**Kết quả**: Entropy(S) = 1.

## Bước 2: Tính Information Gain để chọn nút gốc

Công thức Information Gain:  
Gain(S, A) = Entropy(S) - Σ ( |Sv|/|S| * Entropy(Sv) )  
Trong đó, v là các giá trị của thuộc tính A, Sv là tập con của S với A = v.

Ta tính Gain cho từng thuộc tính: Hài, Ngươi nổi tiếng, Theo trend, Có tranh luận.

### Thuộc tính "Hài"

- **Giá trị**: Có (2 mẫu), Không (4 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 2, 6).  
  - p+ = 1/2 = 0.5, p- = 0.5.  
  - Entropy(S_Có) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- S_Không: [2 Đúng, 2 Sai] (mẫu 1, 3, 4, 5).  
  - p+ = 2/4 = 0.5, p- = 0.5.  
  - Entropy(S_Không) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- I(Hài) = (2/6) * 1 + (4/6) * 1 = 0.333 + 0.667 = 1.
- Gain(S, Hài) = 1 - 1 = 0.

### Thuộc tính "Ngươi nổi tiếng"

- **Giá trị**: Có (3 mẫu), Không (3 mẫu).
- S_Có: [1 Đúng, 2 Sai] (mẫu 4, 5, 6).  
  - p+ = 1/3 ≈ 0.333, p- = 2/3 ≈ 0.667.  
  - Entropy(S_Có) = -0.333 * log2(0.333) - 0.667 * log2(0.667) ≈ 0.918.
- S_Không: [2 Đúng, 1 Sai] (mẫu 1, 2, 3).  
  - p+ = 2/3 ≈ 0.667, p- = 1/3 ≈ 0.333.  
  - Entropy(S_Không) = -0.667 * log2(0.667) - 0.333 * log2(0.333) ≈ 0.918.
- I(Người nổi tiếng) = (3/6) * 0.918 + (3/6) * 0.918 = 0.918.
- Gain(S, Ngươi nổi tiếng) = 1 - 0.918 = 0.082.

### Thuộc tính "Theo trend"

- **Giá trị**: Có (5 mẫu), Không (1 mẫu).
- S_Có: [3 Đúng, 2 Sai] (mẫu 1, 2, 3, 4, 6).  
  - p+ = 3/5 = 0.6, p- = 2/5 = 0.4.  
  - Entropy(S_Có) = -0.6 * log2(0.6) - 0.4 * log2(0.4) ≈ 0.971.
- S_Không: [0 Đúng, 1 Sai] (mẫu 5).  
  - p+ = 0, p- = 1.  
  - Entropy(S_Không) = -0 * log2(0) - 1 * log2(1) = 0.
- I(Theo trend) = (5/6) * 0.971 + (1/6) * 0 = 0.811.
- Gain(S, Theo trend) = 1 - 0.811 = 0.189.

### Thuộc tính "Có tranh luận"

- **Giá trị**: Có (2 mẫu), Không (4 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 3, 5).  
  - p+ = 1/2 = 0.5, p- = 0.5.  
  - Entropy(S_Có) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- S_Không: [2 Đúng, 2 Sai] (mẫu 1, 2, 4, 6).  
  - p+ = 2/4 = 0.5, p- = 0.5.  
  - Entropy(S_Không) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- I(Có tranh luận) = (2/6) * 1 + (4/6) * 1 = 0.333 + 0.667 = 1.
- Gain(S, Có tranh luận) = 1 - 1 = 0.

## Bước 3: So sánh Information Gain

- **Hài**: 0
- **Ngươi nổi tiếng**: 0.082
- **Theo trend**: 0.189
- **Có tranh luận**: 0

Thuộc tính **Theo trend** có Information Gain cao nhất (0.189).

**Nút gốc** của cây quyết định là **Theo trend**.

# Phân tích và xây dựng cây quyết định cho nhánh "Theo trend = Có"

| Mẫu | Hài   | Ngươi nổi tiếng | Theo trend | Có tranh luận | THÍCH |
|------|-------|-----------------|------------|---------------|-------|
| 1    | Không | Không           | Có         | Không         | Sai   |
| 2    | Có    | Không           | Có         | Không         | Đúng  |
| 3    | Không | Không           | Có         | Có            | Đúng  |
| 4    | Không | Có              | Có         | Không         | Đúng  |
| 6    | Có    | Có              | Có         | Không         | Sai   |

- **Tổng số mẫu**: 5.
- **THÍCH = Đúng**: 3 (mẫu 2, 3, 4).
- **THÍCH = Sai**: 2 (mẫu 1, 6).
- Tỷ lệ p+ (THÍCH = Đúng) = 3/5 = 0.6.
- Tỷ lệ p- (THÍCH = Sai) = 2/5 = 0.4.

## Bước 1: Tính Entropy của tập dữ liệu \( S_{\text{Theo trend = Có}} \)

Công thức Entropy:  
Entropy(S) = -p+ * log2(p+) - p- * log2(p-)

Thay số:  
Entropy(S) = -0.6 * log2(0.6) - 0.4 * log2(0.4)  
- log2(0.6) ≈ -0.737, log2(0.4) ≈ -1.322  
- -0.6 * (-0.737) ≈ 0.442  
- -0.4 * (-1.322) ≈ 0.529  
- Entropy(S) ≈ 0.442 + 0.529 = 0.971

**Kết quả**: Entropy(S_Theo trend = Có) ≈ 0.971.

## Bước 2: Tính Information Gain để chọn thuộc tính tiếp theo

Công thức Information Gain:  
Gain(S, A) = Entropy(S) - Σ ( |Sv|/|S| * Entropy(Sv) )  
Trong đó, v là các giá trị của thuộc tính A, Sv là tập con của S với A = v.

Ta tính Gain cho từng thuộc tính: Hài, Ngươi nổi tiếng, Có tranh luận.

### Thuộc tính "Hài"

- **Giá trị**: Có (2 mẫu), Không (3 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 2, 6).  
  - p+ = 1/2 = 0.5, p- = 0.5.  
  - Entropy(S_Có) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- S_Không: [2 Đúng, 1 Sai] (mẫu 1, 3, 4).  
  - p+ = 2/3 ≈ 0.667, p- = 1/3 ≈ 0.333.  
  - Entropy(S_Không) = -0.667 * log2(0.667) - 0.333 * log2(0.333) ≈ 0.918.
- I(Hài) = (2/5) * 1 + (3/5) * 0.918 ≈ 0.4 + 0.551 = 0.951.
- Gain(S, Hài) = 0.971 - 0.951 ≈ 0.02.

### Thuộc tính "Ngươi nổi tiếng"

- **Giá trị**: Có (2 mẫu), Không (3 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 4, 6).  
  - Entropy(S_Có) = 1.
- S_Không: [2 Đúng, 1 Sai] (mẫu 1, 2, 3).  
  - Entropy(S_Không) ≈ 0.918.
- I(Người nổi tiếng) = (2/5) * 1 + (3/5) * 0.918 ≈ 0.4 + 0.551 = 0.951.
- Gain(S, Ngươi nổi tiếng) = 0.971 - 0.951 ≈ 0.02.

### Thuộc tính "Có tranh luận"

- **Giá trị**: Có (1 mẫu), Không (4 mẫu).
- S_Có: [1 Đúng, 0 Sai] (mẫu 3).  
  - Entropy(S_Có) = 0.
- S_Không: [2 Đúng, 2 Sai] (mẫu 1, 2, 4, 6).  
  - p+ = 2/4 = 0.5, p- = 0.5.  
  - Entropy(S_Không) = -0.5 * log2(0.5) - 0.5 * log2(0.5) = 1.
- I(Có tranh luận) = (1/5) * 0 + (4/5) * 1 = 0.8.
- Gain(S, Có tranh luận) = 0.971 - 0.8 = 0.171.

## Bước 3: So sánh Information Gain

- **Hài**: 0.02
- **Ngươi nổi tiếng**: 0.02
- **Có tranh luận**: 0.171

Thuộc tính **Có tranh luận** có Information Gain cao nhất (0.171).

Thuộc tính tiếp theo để phân nhánh từ "Theo trend = Có" là **Có tranh luận**.

# Phân tích và xây dựng cây quyết định cho nhánh "Theo trend = Có, Có tranh luận = Không"

## Dữ liệu ban đầu cho nhánh

| Mẫu | Hài   | Ngươi nổi tiếng | Theo trend | Có tranh luận | THÍCH |
|------|-------|-----------------|------------|---------------|-------|
| 1    | Không | Không           | Có         | Không         | Sai   |
| 2    | Có    | Không           | Có         | Không         | Đúng  |
| 4    | Không | Có              | Có         | Không         | Đúng  |
| 6    | Có    | Có              | Có         | Không         | Sai   |

- **Tổng số mẫu**: 4.
- **THÍCH = Đúng**: 2 (mẫu 2, 4).
- **THÍCH = Sai**: 2 (mẫu 1, 6).
- Tỷ lệ p+ (THÍCH = Đúng) = 2/4 = 0.5.
- Tỷ lệ p- (THÍCH = Sai) = 2/4 = 0.5.

## Bước 1: Tính Entropy của tập dữ liệu

Công thức Entropy:  
Entropy(S) = -p+ * log2(p+) - p- * log2(p-)

Thay số:  
Entropy(S) = -0.5 * log2(0.5) - 0.5 * log2(0.5)  
= -0.5 * (-1) - 0.5 * (-1) = 0.5 + 0.5 = 1

**Kết quả**: Entropy(S_Theo trend = Có, Có tranh luận = Không) = 1.

## Bước 2: Tính Information Gain để chọn thuộc tính tiếp theo

Công thức Information Gain:  
Gain(S, A) = Entropy(S) - Σ ( |Sv|/|S| * Entropy(Sv) )

Ta tính Gain cho các thuộc tính còn lại: Hài, Ngươi nổi tiếng.

### Thuộc tính "Hài"

- **Giá trị**: Có (2 mẫu), Không (2 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 2, 6).  
  - Entropy(S_Có) = 1.
- S_Không: [1 Đúng, 1 Sai] (mẫu 1, 4).  
  - Entropy(S_Không) = 1.
- I(Hài) = (2/4) * 1 + (2/4) * 1 = 0.5 + 0.5 = 1.
- Gain(S, Hài) = 1 - 1 = 0.

### Thuộc tính "Ngươi nổi tiếng"

- **Giá trị**: Có (2 mẫu), Không (2 mẫu).
- S_Có: [1 Đúng, 1 Sai] (mẫu 4, 6).  
  - Entropy(S_Có) = 1.
- S_Không: [1 Đúng, 1 Sai] (mẫu 1, 2).  
  - Entropy(S_Không) = 1.
- I(Người nổi tiếng) = (2/4) * 1 + (2/4) * 1 = 0.5 + 0.5 = 1.
- Gain(S, Ngươi nổi tiếng) = 1 - 1 = 0.

## Bước 3: So sánh Information Gain

- **Hài**: 0
- **Ngươi nổi tiếng**: 0

Cả hai thuộc tính đều có Gain = 0. Ta chọn **Ngươi nổi tiếng** để phân nhánh tiếp (theo thứ tự ưu tiên).

## Bước 4: Phân nhánh với "Ngươi nổi tiếng"

### Nhánh "Ngươi nổi tiếng = Có"

Dữ liệu: Mẫu 4, 6.

| Mẫu | Hài   | Ngươi nổi tiếng | Theo trend | Có tranh luận | THÍCH |
|------|-------|-----------------|------------|---------------|-------|
| 4    | Không | Có              | Có         | Không         | Đúng  |
| 6    | Có    | Có              | Có         | Không         | Sai   |

- Entropy = 1.
- Thuộc tính còn lại: Hài.
- S_Có: [0 Đúng, 1 Sai] (mẫu 6) → Entropy = 0.
- S_Không: [1 Đúng, 0 Sai] (mẫu 4) → Entropy = 0.
- I(Hài) = (1/2) * 0 + (1/2) * 0 = 0.
- Gain(S, Hài) = 1 - 0 = 1.

Chọn "Hài":  
- Hài = Có → Sai.  
- Hài = Không → Đúng.

### Nhánh "Ngươi nổi tiếng = Không"

Dữ liệu: Mẫu 1, 2.

| Mẫu | Hài   | Ngươi nổi tiếng | Theo trend | Có tranh luận | THÍCH |
|------|-------|-----------------|------------|---------------|-------|
| 1    | Không | Không           | Có         | Không         | Sai   |
| 2    | Có    | Không           | Có         | Không         | Đúng  |

- Entropy = 1.
- Thuộc tính còn lại: Hài.
- S_Có: [1 Đúng, 0 Sai] (mẫu 2) → Entropy = 0.
- S_Không: [0 Đúng, 1 Sai] (mẫu 1) → Entropy = 0.
- I(Hài) = (1/2) * 0 + (1/2) * 0 = 0.
- Gain(S, Hài) = 1 - 0 = 1.

Chọn "Hài":  
- Hài = Có → Đúng.  
- Hài = Không → Sai.

## Cây quyết định cho nhánh này

```
Theo trend = Có, Có tranh luận = Không
├── Ngươi nổi tiếng
│   ├── Có
│   │   ├── Hài
│   │   │   ├── Có → Sai
│   │   │   └── Không → Đúng
│   └── Không
│       ├── Hài
│       │   ├── Có → Đúng
│       │   └── Không → Sai
```


