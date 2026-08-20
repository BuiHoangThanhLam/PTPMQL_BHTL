
1. **`[Route]` (Định tuyến):** Đóng vai trò làm "cổng vào". Bắt đường dẫn URL từ người dùng và chỉ định chính xác Controller cùng Action nào sẽ xử lý.
2. **`Controller` (Xử lý logic):** Đóng vai trò "trung tâm". Nhận dữ liệu từ Route, thực thi logic (truy vấn CSDL, xử lý dữ liệu) và quyết định trả về View nào.
3. **`View` (Giao diện):** Đóng vai trò "đầu ra". Nhận dữ liệu do Controller gửi sang để hiển thị kết quả HTML cho người dùng.

---

### Sơ đồ liên kết

```text
  [URL / Request]
        │
        ▼
   [ Route ] ──(Định vị đúng Controller & Action)──► [ Controller ]
                                                           │
                                             (Trả về View + dữ liệu Model)
                                                           │
                                                           ▼
  [ Response HTML ] ◄──(Hiển thị giao diện)─────────── [ View ]

```

---

### Ví dụ minh họa kết nối

#### 1. Định nghĩa Route & Controller (`Controllers/ProductController.cs`)

Thuộc tính `[Route]` ghép nối đường dẫn URL trực tiếp tới phương thức xử lý trong Controller:

```csharp
[Route("san-pham")] // Route cơ sở (Base Route)
public class ProductController : Controller
{
    // URL hoàn chỉnh: /san-pham/chi-tiet/5
    [HttpGet("chi-tiet/{id:int}")] 
    public IActionResult Detail(int id)
    {
        var product = new Product { Id = id, Name = "Thiết bị điện tử" };

        // Controller gọi View cùng tên (Detail) và truyền dữ liệu 'product' sang
        return View(product); 
    }
}

```

#### 2. Kết nối với View (`Views/Product/Detail.cshtml`)

Theo quy ước (Convention) của MVC, hàm `return View()` ở `ProductController` sẽ tự động tìm đến file giao diện tương ứng theo cấu trúc thư mục:

`Views` ➔ `Product` ➔ `Detail.cshtml`

```cshtml
@model Product

<h1>Chi tiết sản phẩm</h1>
<p>Mã: @Model.Id</p>
<p>Tên: @Model.Name</p>

```

---

### Quy tắc ghép nối cần nhớ

* **Route ➔ Controller:** Tên `[Route]` quyết định URL mà người dùng cần gõ để kích hoạt Controller/Action tương ứng.
* **Controller ➔ View:** Hàm `return View()` trong Action sẽ tự động tìm file `.cshtml` có **tên trùng với tên Action** và nằm trong thư mục `Views/[TênController]/`.