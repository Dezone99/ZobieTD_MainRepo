# 📦 Hướng dẫn sử dụng Unity Memory Profiler

**Memory Profiler** là công cụ chính thức của Unity dùng để kiểm tra, phân tích và so sánh bộ nhớ (RAM/GPU) trong game, giúp phát hiện các vấn đề như:
- Tài nguyên (asset) chưa được giải phóng
- Biến `static` hoặc `reference` giữ object không cần thiết
- Texture/Sprite/Prefab bị leak
- Sự chênh lệch bộ nhớ giữa các scene, thời điểm

![image](https://github.com/user-attachments/assets/24ed5bf7-3262-4cae-ae4c-b7a59d009747)

---

## 🚀 1. Cài đặt Memory Profiler

1. Mở Unity → `Window > Package Manager`
2. Ở góc trái, chọn **Unity Registry**
3. Tìm `Memory Profiler` → **Install**
4. Mở công cụ: `Window > Analysis > Memory Profiler`

---

## 🧪 2. Cách chụp snapshot bộ nhớ

- Mở Memory Profiler
- Nhấn **Capture New Snapshot**
  - Chọn Editor hoặc Play Mode tùy thời điểm bạn muốn chụp
  - Snapshot sẽ được lưu trong danh sách bên trái

📌 **Tip:** Chụp nhiều snapshot ở các thời điểm khác nhau để so sánh:
- Sau khi load bundle
- Sau khi unload bundle
- Sau khi chuyển scene...

---

## 🔍 3. Phân tích snapshot

Sau khi mở snapshot:
- Chọn tab **"All Of Memory"**
- Bên trái là danh sách object/group:
  - `Unity Objects`: Asset như Texture2D, Sprite, GameObject, Animator...
  - `Managed Objects`: Biến C#, instance class của bạn
  - `Native Allocations`: Đa phần do Unity Engine sử dụng
- Sử dụng thanh **Search** để tìm object cụ thể

---

## 🔎 4. Kiểm tra xem object có bị giữ không

1. Chọn 1 object nghi ngờ leak (ví dụ: Sprite, Texture2D)
2. Nhấn nút `References` ở góc phải
3. **Xem chuỗi tham chiếu (reference chain)** để biết:
   - Ai đang giữ object này?
   - Có phải do biến `static`, `field`, `scene` hay `AssetBundle`?

📌 Nếu không còn ai giữ → object sẽ bị GC (xóa) sau khi gọi `Resources.UnloadUnusedAssets() + GC.Collect()`

---

## 🔁 5. So sánh 2 snapshot

1. Click chuột phải vào snapshot đầu → chọn `Compare With...`
2. Chọn snapshot thứ hai để so sánh
3. Chuyển sang tab `Diff`

Tại đây bạn có thể xem:
- Object nào được **tạo mới (Added)**
- Object nào đã bị **xóa (Removed)**
- Memory tăng hoặc giảm bao nhiêu

![image](https://github.com/user-attachments/assets/caed6b59-8043-48e9-a08f-e1b29625f7eb)

---

## 📈 6. Phân tích theo nhóm

- `Graphics > Texture2D`: kiểm tra các texture đang chiếm GPU RAM
- `Unity Objects > Sprite`: kiểm tra số sprite còn sống
- `Managed Objects`: theo dõi object C# như `GameData`, `CharacterInfo`

---

## 🧠 7. Mẹo kiểm tra leak nhanh

- Gọi `Resources.UnloadUnusedAssets()` và `System.GC.Collect()` trước khi chụp snapshot để ép giải phóng memory
- Dùng `Resources.FindObjectsOfTypeAll<T>()` trong Editor để liệt kê object còn tồn tại
- Dùng snapshot diff để xác định asset không bị giải phóng dù đã gọi unload

---

## ✅ Kết luận

Memory Profiler là công cụ mạnh giúp:
- Tối ưu RAM cho mobile
- Phát hiện asset không bị giải phóng
- Giảm nguy cơ crash do out-of-memory
- Kiểm tra Sprite, Texture, AssetBundle đang còn sống hay không

---

## 📚 Tài liệu tham khảo

- [Unity Manual – Memory Profiler](https://docs.unity3d.com/Packages/com.unity.memoryprofiler@latest)
- [Unity Blog – Deep dive into memory analysis](https://blog.unity.com/technology/understanding-memory-in-unity)

