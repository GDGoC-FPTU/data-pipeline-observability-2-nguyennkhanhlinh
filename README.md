[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112873&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** khanhlinhkl2004@gmail.com
**Name:** Nguyễn Khánh Linh

---

## Mo ta

Bai lab xay dung mot pipeline ETL tu dong va tim hieu tam quan trong cua chat luong du lieu
(Data Observability) doi voi AI Agent. Nhung gi da lam:

- **Extract:** Doc du lieu san pham tu file JSON (`raw_data.json`), xu ly truong hop file khong ton tai.
- **Validate:** Loai bo cac record khong hop le (gia <= 0, thieu category) va ghi log so record giu lai / loai bo.
- **Transform:** Tinh `discounted_price` (giam 10%), chuan hoa `category` ve Title Case, them cot `processed_at` (timestamp).
- **Load:** Luu ket qua ra `processed_data.csv`.
- **Stress Test:** Chay `agent_simulation.py` voi du lieu sach va du lieu rac de so sanh, ghi nhan xet vao `experiment_report.md`.

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
# 1. Tao file du lieu rac
python generate_garbage.py

# 2. Chay Agent voi ca du lieu sach (processed_data.csv) va du lieu rac (garbage_data.csv)
python agent_simulation.py
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── raw_data.json            # Du lieu dau vao
├── processed_data.csv       # Output cua pipeline
├── generate_garbage.py      # Tao du lieu "rac" cho 
```

---

## Ket qua

- **Records xu ly:** 5 records dau vao -> 3 records hop le, 2 records bi loai.
  - id 3: bi loai vi `price <= 0`.
  - id 4: bi loai vi thieu `category`.
- **Output:** `processed_data.csv` chua 3 records da duoc transform (discounted_price, Title Case category, processed_at).
- **Stress Test:**
  - Voi du lieu sach -> Agent tra loi dung: "Laptop at $1200".
  - Voi du lieu rac -> Agent bi danh lua boi outlier: "Nuclear Reactor at $999999".
- **Ket luan:** Chat luong du lieu quyet dinh truc tiep chat luong cau tra loi cua AI Agent (Garbage In, Garbage Out).
