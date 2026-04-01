## 🔍 คำตอบคำถามจาก Lab

### 1. ถ้ามีข้อมูล Ground Truth จริง คิดว่า Accuracy ของโมเดลนี้จะอยู่ในระดับใด? เพราะอะไร?
ตอบ คาดว่า ROC-AUC อยู่ในช่วง 0.70–0.80** (Moderate-Good)
เพราะ โมเดล WLC ไม่ใช่ Machine Learning จึงไม่ถูก optimize กับข้อมูล   ceiling ต่ำกว่า Random Forest
ปัจจัยหลายตัว (ERA5, MODIS) มี resolution หยาบ ไป ลด spatial precision

### 2. ปัจจัยที่ไม่ได้ใส่ในโมเดลแต่น่าจะสำคัญมีอะไรบ้าง? ทำไมถึงไม่ใส่?
ตอบ
| ปัจจัย | เหตุผลที่ไม่ใส่ |
|--------|----------------|
| Soil Type / Permeability | ข้อมูล soil ใน GEE ครอบคลุมไทยไม่ดี (SoilGrids resolution 250m) |
| Drainage Infrastructure | ไม่มีข้อมูลดิจิทัลระดับพื้นที่ย่อย |
| Antecedent Soil Moisture | ERA5 Soil Moisture ซ้อนทับกับ Rainfall factor บางส่วน |
| Population Density | สำคัญถ้าโมเดลเน้น Risk ต่อคน ไม่ใช่ Hazard |

### 3. มเดลนี้ใช้ได้ดีในระดับ Scale ใด (ตำบล / อำเภอ / จังหวัด) เพราะอะไร?
ตอบ ระดับอำเภอ เพราะ: ERA5 rain data มี resolution ~9 km  MODIS LULC 500m  precision ตำบลไม่พอ และ SRTM 30m ดีพอสำหรับระดับอำเภอขึ้นไป

### 4. ถ้าต้องนำโมเดลนี้ไปอัปเดตปีละครั้ง ขั้นตอนใดที่ต้องทำซ้ำและขั้นตอนใดที่คงที่?
ตอบ
| ขั้นตอน | สถานะ | เหตุผล |
|---------|-------|--------|
| โหลด Slope, DEM, TWI | **คงที่** | ภูมิประเทศเปลี่ยนช้ามาก |
| โหลด Distance to River | **คงที่** | HydroSHEDS ไม่เปลี่ยน |
| อัปเดต Rainfall (ERA5) | **ทำซ้ำ** | ฝนเปลี่ยนทุกปี |
| อัปเดต LULC (MODIS) | **ทำซ้ำ** | การใช้ที่ดินเปลี่ยน |
| อัปเดต JRC GSW | **ทำซ้ำ** | มีข้อมูลปีใหม่ |
| Weights / Framework | **คงที่** | ต้องทำ AHP ใหม่ถ้าเปลี่ยน |
| Validation | **ทำซ้ำ** | เปรียบเทียบกับน้ำท่วมปีล่าสุด |

---
