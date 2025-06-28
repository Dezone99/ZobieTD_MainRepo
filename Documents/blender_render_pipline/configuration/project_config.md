# ⚙️ Hướng dẫn sử dụng `project_config.py`

Tệp `project_config.py` là script dạng dictionary Python dùng để cấu hình cho pipeline render tự động trong Blender. Tệp này được lưu dưới dạng **Text datablock** trong Blender, và sẽ được đọc bởi các tool như `render_dispatcher.py`.

---

## 🔖 Cấu trúc chính

```python
project_config = {
    "project_type": "multi_object",
    "output_folder": "bin",
    "collections": [...],
    "actions": [...],
    "armature_name": "",
    "postprocess": { ... },
    "post_renderer_internal": {
        "pack_sprite_sheet": { ... }
    }
}
```

---

## 📌 Các trường cấu hình

### `project_type` (bắt buộc)

- `"static"`: render từng object trong collection, áp dụng cho tile/map
- `"armature"`: render theo action của 1 armature object
- `"multi_object"`: render nhiều object, mỗi object có action cùng tên

---

### `output_folder` (tuỳ chọn)

- Thư mục xuất ảnh/video sau render
- Nếu không khai báo thì mặc định là `"bin"`

---

### `collections` (chỉ áp dụng cho `"static"`)

- Danh sách collection trong scene sẽ được render từng object bên trong

---

### `actions` (áp dụng cho `"armature"` và `"multi_object"`)

- Với `multi_object`: **bắt buộc** phải liệt kê các action rõ ràng
- Với `armature`: nếu không khai báo → tự động lấy các action theo pattern

---

### `armature_name` (chỉ áp dụng cho `"armature"`)

- Tên object armature trong scene

---

## 🎨 `postprocess` – Xử lý hậu kỳ sau khi render từng scene

Mỗi scene có thể gắn 1 tool hậu kỳ riêng. Mỗi tool gồm:

```python
"postprocess": {
    "TênScene": {
        "method": "tên_hàm_xử_lý",
        "args": {
            "glow_power": 0.76,
            "overlay_fac": 0.2,
            "glow_gain": 0.8,
            "glow_stack_fac_list": "",
            "overlay_color": "1.0,0.906,0.130,1.0"
        }
    }
}
```

- `method`: tên hàm xử lý hậu kỳ
- `args`: tham số truyền vào tool

---

## 🧪 `post_renderer_internal.pack_sprite_sheet` – Gộp các frame PNG thành sprite sheet

Dùng để tối ưu ảnh sau render. Có thể cấu hình riêng theo từng scene hoặc theo mặc định `"*"`.

### Cấu trúc:

```python
"pack_sprite_sheet": {
    "VFX": {
        "pattern": "frame_*.png",
        "is_debug": True,
        "threshold": 16,
        "crop_mode": "brightness",
        "blend_mode": "ADD"
    },
    "*": {
        "pattern": "frame_*.png",
        "is_debug": True,
        "threshold": 5,
        "crop_mode": "alpha",
        "blend_mode": "NORMAL"
    }
}
```

### Giải thích tham số:

| Tham số         | Kiểu     | Mô tả                                                                 |
|------------------|----------|------------------------------------------------------------------------|
| `pattern`        | string   | Pattern filter ảnh PNG cần gom                                       |
| `is_debug`       | bool     | Nếu True thì xuất ảnh debug có hiển thị khung và label               |
| `threshold`      | int      | Ngưỡng cắt theo độ sáng hoặc alpha                                    |
| `crop_mode`      | string   | `"alpha"`, `"brightness"`                                             |
| `blend_mode`     | string   | `"NORMAL"`, `"ADD"`, dùng để xác định chế độ blend khi preview/game |

---

## 📎 Tài liệu liên quan

- [Tool: render_dispatcher.py](https://swarm-dezone99.ddns.net/files/zbietd/blender/zbie_td2025/.synclab/main/render_dispatcher.py)
- [Tool: bloom_background_remove](https://swarm-dezone99.ddns.net/files/zbietd/blender/zbie_td2025/.synclab/tools/bloom_background_remove.py)
- [Tool: make_sprite_sheet](https://swarm-dezone99.ddns.net/files/zbietd/blender/zbie_td2025/.synclab/tools/unblend/make_sprite_sheet.md)
