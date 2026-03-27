# Lab 3: Machine Learning สำหรับการจำแนกการใช้ที่ดิน
## พื้นที่ศึกษา: จังหวัดระยอง | ข้อมูล: Landsat 8 SR
**ภม.338 Geographic Data Science**

---

## คำถามท้าย README

### 1. ถ้าเพิ่ม Training Samples 2 เท่า OA จะเพิ่มแค่ไหน?
จาก Learning Curve: การเพิ่มจาก 80 → 160 samples/class ให้ผลตอบแทนที่ลดลง (Diminishing Returns) — OA มักเพิ่มขึ้น 1-3% เนื่องจากโมเดล RF converge แล้วที่ประมาณ 80 samples/class สำหรับ 5 Classes

### 2. Spatial Autocorrelation ในข้อมูล Training มีผลต่อความแม่นยำที่รายงานอย่างไร?
Training Points ที่อยู่ใกล้กันใน Polygon เดียวกันมี Spectral Values คล้ายกัน (Spatially Autocorrelated) การ Random Split ทำให้ Train/Test Points อยู่ใกล้กัน → OA ที่รายงาน **สูงกว่า OA จริง** เมื่อทดสอบกับพื้นที่อื่น แก้ได้ด้วย Spatial Cross-validation (Block CV)

### 3. Class ใดที่โมเดลทำได้แย่ที่สุด — แก้ได้ด้วยวิธีใดบ้าง?
- **Wetland** มักสับสนกับ Water และ Agriculture
- แก้ได้โดย: (1) เพิ่ม Temporal Features (NDVI ช่วงต่างๆ), (2) เพิ่ม SAR Data (Sentinel-1) ซึ่งแยก Wetland ได้ดีกว่า Optical, (3) เพิ่ม Training Samples เฉพาะ Class นี้

### 4. ถ้าต้องทำซ้ำ Lab นี้สำหรับพื้นที่อื่น อะไรคือสิ่งที่ต้องเปลี่ยน และอะไรที่ใช้ซ้ำได้?
| เปลี่ยน | ใช้ซ้ำได้ |
|---------|----------|
| AOI boundary | Code Structure ทั้งหมด |
| Training Polygons (Digitize ใหม่) | Spectral Indices Functions |
| Class definitions (ปรับตามบริบท) | Model Training Pipeline |
| Date range (ให้เหมาะกับฤดูกาล) | Visualization & Export Code |

---

## ข้อจำกัดที่พบ
1. **Cloud Cover**: แม้ใช้ Median Composite แต่บางพื้นที่อาจมี Artifact จาก Cloud Shadow
2. **Mixed Pixels**: ที่ Resolution 30m ขอบเขต Class ไม่ชัดเจน
3. **Temporal Mismatch**: ใช้ข้อมูล 2020-2023 แต่สภาพพื้นที่เปลี่ยนแปลงอยู่ตลอด
4. **Training Data Bias**: Polygon วาดเองอาจมี Bias จากประสบการณ์ผู้ Digitize

---
*ภม.338 Geographic Data Science | จังหวัดระยอง | Landsat 8 SR | GEE Python API*
