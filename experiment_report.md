# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600108
**Name:** Lê Trung Anh Quốc
**Date:** 15/4/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Testing with CLEAN data:Agent: Based on my data, the best choice is Laptop at $1200. | 10| 
| Garbage Data (`garbage_data.csv`) | Testing with GARBAGE data: Agent: Based on my data, the best choice is Nuclear Reactor at $999999. |10 | 

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent sai vì pipeline không có bước “lọc rác” trước khi suy luận, nên rule chọn kết quả bị nhiễu bởi dữ liệu bẩn.

Cụ thể, trong code:

Với câu hỏi chứa electronic, agent lọc category == electronics.
Sau đó chọn dòng có price lớn nhất bằng idxmax().
Nghĩa là định nghĩa “best” = “đắt nhất”.
Trong file garbage_data.csv, nhóm electronics có:

Laptop, 1200
Nuclear Reactor, 999999 (giá trị phi thực tế, dữ liệu rác)
Vì vậy agent trả về Nuclear Reactor là hệ quả tất yếu của logic hiện tại, không phải do model “hiểu sai ngữ nghĩa”.

Ngoài ra dataset còn các lỗi chất lượng làm tăng rủi ro trả lời sai:

price = "ten dollars" (sai kiểu dữ liệu số)
id trùng (id=1 xuất hiện 2 lần)
bản ghi thiếu id, thiếu category
---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)

Đồng ý.Prompt tốt chỉ giúp hỏi đúng ,trả lời đúng trọng tâm chủ đề, dữ liệu tốt mới quyết định câu trả lời đúng. Dữ liệu rác thì prompt hay vẫn có thể trả lời sai.
