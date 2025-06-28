# 📦 animation_bundle.json

Tệp `animation_bundle.json` mô tả toàn bộ thông tin animation theo từng frame, bao gồm các sprite sheet được sử dụng, cell bounding box, origin và thứ tự các layer trong mỗi frame.

---

## 🧩 Cấu trúc tổng quan

```json
{
  "sheets": [...],
  "frames": [...]
}
```

---

## 🗂️ sheets (sprite sheets)

Danh sách các sprite sheet được sử dụng trong bundle.

### Mỗi `sheet` gồm:

| Trường         | Kiểu dữ liệu | Mô tả                                 |
|----------------|--------------|----------------------------------------|
| `sheet_id`     | int          | ID duy nhất của sprite sheet          |
| `layer`        | string       | Tên layer tương ứng với object hoặc scene |
| `sheet_path`   | string       | Đường dẫn đến file sprite sheet PNG   |
| `sheet_width`  | int          | Chiều rộng của sheet                  |
| `sheet_height` | int          | Chiều cao của sheet                   |
| `cells`        | array        | Danh sách các vùng crop (sprites)     |

#### Mỗi `cell` gồm:

| Trường | Kiểu dữ liệu | Mô tả                         |
|--------|--------------|-------------------------------|
| `id`   | int          | ID của sprite (frame)         |
| `x`    | int          | Tọa độ X trong sheet          |
| `y`    | int          | Tọa độ Y trong sheet          |
| `w`    | int          | Chiều rộng                    |
| `h`    | int          | Chiều cao                     |

---

## 🎞️ frames (animation frames)

Mỗi phần tử trong `frames` tương ứng với một frame trong animation timeline.

### Mỗi `frame` gồm:

| Trường     | Kiểu dữ liệu | Mô tả                         |
|------------|--------------|-------------------------------|
| `index`    | int          | Chỉ số frame                  |
| `layers`   | array        | Danh sách layer trong frame   |

#### Mỗi `layer` gồm:

| Trường        | Kiểu dữ liệu     | Mô tả                                     |
|---------------|------------------|-------------------------------------------|
| `sheet_id`    | int              | ID của sprite sheet sử dụng               |
| `cell_id`     | int \| null      | ID sprite trong sheet, `null` nếu không có |
| `origin`      | float[2]         | Tọa độ gốc của sprite (x, y)              |
| `z_index`     | int              | Thứ tự vẽ layer (nhỏ hơn vẽ trước)       |

---

## 💡 Ghi chú
- `z_index` là thứ tự logic của layer trong mỗi frame, được tính sau khi sort theo `z_camspace`.
- `cell_id = null` cho biết frame đó không có nội dung hiển thị (thường dùng cho VFX chưa kích hoạt).
- Hệ thống render sẽ căn theo `origin` để đặt đúng vị trí của sprite.

---

## 📎 Tài liệu liên quan

- [ERD sơ đồ quan hệ animation_bundle.drawio](../data_structure/animation_bundle_erd_with_zindex_uniform.drawio.xml)
- [Tool: make_animation_bundle.py](../tools/make_animation_bundle.md)
