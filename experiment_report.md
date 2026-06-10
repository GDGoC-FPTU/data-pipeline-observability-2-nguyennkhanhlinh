# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600856
**Name:** Nguyễn Khánh Linh
**Date:** 10/06/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Based on my data, the best choice is Laptop at $1200. |  | Du lieu da qua validate/transform nen ket qua chinh xac, hop ly. |
| Garbage Data (`garbage_data.csv`) | Based on my data, the best choice is Nuclear Reactor at $999999. | | Agent bi danh lua boi outlier, dua ra san pham vo ly. |

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

Agent tra loi sai khi dung Garbage Data vi du lieu dau vao chua duoc lam sach, chua qua buoc
validate va transform nhu trong pipeline ETL. Cu the:

- **Outliers (gia tri ngoai le):** Ban ghi "Nuclear Reactor" co gia 999999 la mot outlier cuc lon.
  Logic cua Agent chi don gian chon san pham co gia cao nhat (`price.idxmax()`), nen no "tin tuong"
  ngay outlier nay va dua ra cau tra loi vo ly thay vi san pham thuc su tot nhat (Laptop).

- **Wrong data types (sai kieu du lieu):** Ban ghi "Broken Chair" co gia la chuoi `'ten dollars'`
  thay vi so. Khi so sanh hoac tinh toan tren cot price, du lieu kieu hon hop nay co the gay loi
  hoac khien ket qua so sanh khong dang tin cay.

- **Duplicate IDs (ID trung lap):** Co hai ban ghi cung `id = 1` (Laptop va Banana). Dieu nay pha vo
  tinh duy nhat cua khoa, khien viec tra cuu/ghep noi du lieu bi nham lan va dem trung.

- **Null values (gia tri rong):** Ban ghi "Ghost Item" co `id` va `category` la null. Nhung gia tri
  thieu nay lam cac phep loc theo category that bai va co the gay loi runtime.

Tom lai, mot AI Agent (kieu RAG) khong tu danh gia duoc chat luong du lieu — no chi tin tuong vao
nhung gi duoc cung cap. Neu "kho tri thuc" chua du rac, Agent se khuech dai loi do va dua ra ket qua
sai lech (garbage in, garbage out).

---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)

Dong y. Du prompt co duoc viet tot den dau, neu du lieu dau vao la "rac" thi Agent van se dua ra
cau tra loi sai (nhu vi du chon "Nuclear Reactor"). Mot prompt hay chi giup dinh huong cach tra loi,
nhung khong the tu sua loi du lieu sai, outlier hay gia tri null. Nguoc lai, khi du lieu da duoc lam
sach qua pipeline ETL (validate + transform), ngay ca logic don gian cung cho ket qua chinh xac.
Vi vay, dau tu vao chat luong du lieu va kha nang observability cua pipeline quan trong hon viec chi
toi uu prompt: **Garbage In, Garbage Out**.
