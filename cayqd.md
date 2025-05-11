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
- Tỷ lệ \( p_+ \) (THÍCH = Đúng) = \( \frac{3}{6} = 0.5 \).
- Tỷ lệ \( p_- \) (THÍCH = Sai) = \( \frac{3}{6} = 0.5 \).

## Bước 1: Tính Entropy của tập dữ liệu ban đầu (S)

Công thức Entropy:

\[
\text{Entropy}(S) = -p_+ \log_2 p_+ - p_- \log_2 p_-
\]

Thay số:

\[
\text{Entropy}(S) = -0.5 \log_2 0.5 - 0.5 \log_2 0.5
\]

\[
= -0.5 \cdot (-1) - 0.5 \cdot (-1) = 0.5 + 0.5 = 1
\]

**Kết quả**: \(\text{Entropy}(S) = 1\).

## Bước 2: Tính Information Gain để chọn nút gốc

Công thức Information Gain:

\[
\text{Gain}(S, A) = \text{Entropy}(S) - \sum_{v \in \text{Values}(A)} \frac{|S_v|}{|S|} \cdot \text{Entropy}(S_v)
\]

Ta tính Gain cho từng thuộc tính: Hài, Ngươi nổi tiếng, Theo trend, Có tranh luận.

### Thuộc tính "Hài"

- **Giá trị**: Có (2 mẫu), Không (4 mẫu).
- \( S_{\text{Có}} \): [1 Đúng, 1 Sai] (mẫu 2, 6).
  - \( p_+ = \frac{1}{2} = 0.5 \), \( p_- = 0.5 \).
  - \[
    \text{Entropy}(S_{\text{Có}}) = -0.5 \log_2 0.5 - 0.5 \log_2 0.5 = 1
    \]
- \( S_{\text{Không}} \): [2 Đúng, 2 Sai] (mẫu 1, 3, 4, 5).
  - \( p_+ = \frac{2}{4} = 0.5 \), \( p_- = 0.5 \).
  - \[
    \text{Entropy}(S_{\text{Không}}) = -0.5 \log_2 0.5 - 0.5 \log_2 0.5 = 1
    \]
- \[
  I(\text{Hài}) = \frac{2}{6} \cdot 1 + \frac{4}{6} \cdot 1 = 0.333 + 0.667 = 1
  \]
- \[
  \text{Gain}(S, \text{Hài}) = 1 - 1 = 0
  \]

### Thuộc tính "Ngươi nổi tiếng"

- **Giá trị**: Có (3 mẫu), Không (3 mẫu).
- \( S_{\text{Có}} \): [1 Đúng, 2 Sai] (mẫu 4, 5, 6).
  - \( p_+ = \frac{1}{3} \approx 0.333 \), \( p_- = \frac{2}{3} \approx 0.667 \).
  - \[
    \text{Entropy}(S_{\text{Có}}) = -0.333 \log_2 0.333 - 0.667 \log_2 0.667 \approx 0.918
    \]
- \( S_{\text{Không}} \): [2 Đúng, 1 Sai] (mẫu 1, 2, 3).
  - \( p_+ = \frac{2}{3} \approx 0.667 \), \( p_- = \frac{1}{3} \approx 0.333 \).
  - \[
    \text{Entropy}(S_{\text{Không}}) = -0.667 \log_2 0.667 - 0.333 \log_2 0.333 \approx 0.918
    \]
- \[
  I(\text{Người nổi tiếng}) = \frac{3}{6} \cdot 0.918 + \frac{3}{6} \cdot 0.918 = 0.918
  \]
- \[
  \text{Gain}(S, \text{Người nổi tiếng}) = 1 - 0.918 = 0.082
  \]

### Thuộc tính "Theo trend"

- **Giá trị**: Có (5 mẫu), Không (1 mẫu).
- \( S_{\text{Có}} \): [3 Đúng, 2 Sai] (mẫu 1, 2, 3, 4, 6).
  - \( p_+ = \frac{3}{5} = 0.6 \), \( p_- = \frac{2}{5} = 0.4 \).
  - \[
    \text{Entropy}(S_{\text{Có}}) = -0.6 \log_2 0.6 - 0.4 \log_2 0.4 \approx 0.971
    \]
- \( S_{\text{Không}} \): [0 Đúng, 1 Sai] (mẫu 5).
  - \( p_+ = 0 \), \( p_- = 1 \).
  - \[
    \text{Entropy}(S_{\text{Không}}) = -0 \log_2 0 - 1 \log_2 1 = 0
    \]
- \[
  I(\text{Theo trend}) = \frac{5}{6} \cdot 0.971 + \frac{1}{6} \cdot 0 = 0.811
  \]
- \[
  \text{Gain}(S, \text{Theo trend}) = 1 - 0.811 = 0.189
  \]

### Thuộc tính "Có tranh luận"

- **Giá trị**: Có (2 mẫu), Không (4 mẫu).
- \( S_{\text{Có}} \): [1 Đúng, 1 Sai] (mẫu 3, 5).
  - \( p_+ = \frac{1}{2} = 0.5 \), \( p_- = 0.5 \).
  - \[
    \text{Entropy}(S_{\text{Có}}) = -0.5 \log_2 0.5 - 0.5 \log_2 0.5 = 1
    \]
- \( S_{\text{Không}} \): [2 Đúng, 2 Sai] (mẫu 1, 2, 4, 6).
  - \( p_+ = \frac{2}{4} = 0.5 \), \( p_- = 0.5 \).
  - \[
    \text{Entropy}(S_{\text{Không}}) = -0.5 \log_2 0.5 - 0.5 \log_2 0.5 = 1
    \]
- \[
  I(\text{Có tranh luận}) = \frac{2}{6} \cdot 1 + \frac{4}{6} \cdot 1 = 0.333 + 0.667 = 1
  \]
- \[
  \text{Gain}(S, \text{Có tranh luận}) = 1 - 1 = 0
  \]

## Bước 3: So sánh Information Gain

- **Hài**: 0
- **Ngươi nổi tiếng**: 0.082
- **Theo trend**: 0.189
- **Có tranh luận**: 0

Thuộc tính **Theo trend** có Information Gain cao nhất (0.189).

## Kết luận

**Nút gốc** của cây quyết định là **Theo trend**.
