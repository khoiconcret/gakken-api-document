# GAKKEN API DOCUMENT

### 1. Change Master Layout

**URI:** `/gmeds/slide-history/change/master-layout`  
**Method:** `POST`

---

### Request Parameters

| Name             | Type   | Required | Description            |
|------------------|--------|----------|------------------------|
| `master_layout_id` | int/string | Yes      | ID của master layout   |
| `slide_id`         | int/string | Yes      | ID của slide cần thay đổi |
| `current_step`         | int| Yes      | Mục đích để xử lý khi undo và change master layout|

**Response:**
```json
{
    "categories": {
        "01hyacdfj7adef24czzne7wsxa": "G4",
        "01hyacdfjb6n881myy68gr9x4p": "G1",
        "01J1M4S2PS4KH9QR73Q7NC3PA9": "G5",
        "01J1M4S2PS4KH9QR73Q7NC3PAA": "G6",
        "01J1M4S2PS4KH9QR73Q7NC3PAB": "G7",
        "01J1M4S2PS4KH9QR73Q7NC3PAC": "G8",
        "01J1M4S2PS4KH9QR73Q7NC3PAD": "G3",
        "01J1M4S2PS4KH9QR73Q7NC3PAE": "G2"
    },
    "slide_history": {
        "slide_clip_id": "01k5684k9h2kp0stws13xppshh",
        "slide_id": "01k5684kggc5d5mjqpr6rfa5pv",
        "master_layout_id": "5",
        "slide_contents": [
            .......
            {
                "name": "header",
                "order_by": 1,
                "contents_type": 1,
                "contents": null,
                "file_url": null,
                "slide_id": "01k5684kggc5d5mjqpr6rfa5pv",
                "font_size": 100
            },
            {
                "name": "sub_text",
                "order_by": 2,
                "contents_type": 3,
                "contents": null,
                "file_url": null,
                "slide_id": "01k5684kggc5d5mjqpr6rfa5pv",
                "font_size": 100
            },
            .......
        ],
        "narration_text": null,
        "narration_path": null,
        "narration_capacity": null,
        "step": 6,
        "updated_at": "2025-09-15T09:23:59.000000Z",
        "created_at": "2025-09-15T09:23:59.000000Z",
        "id": 9943
    },
    "is_locked": false
}
```

**slide_history**: Thông tin mới nhất của slide được lưu vào history sau mỗi lần change master layout

**slide_history.step**: Dùng để handle việc control button undo

### 2. Undo history

**URI:** `/gmeds/slide-history/undo-step`  
**Method:** `POST`

---

### Request Parameters

| Name        | Type        | Required | Description                       |
|-------------|-------------|----------|-----------------------------------|
| `clip_id`   | int/string  | Yes      | ID của slide clip                 |
| `slide_id`  | int/string  | Yes      | ID của slide                      |
| `step`      | int         | No       | Bước cần undo (nullable)          |

**Response:**
```json
{
    "slide_contents": [
        .......
        {
            "name": "header",
            "order_by": 1,
            "contents_type": 1,
            "contents": null,
            "file_url": null,
            "slide_id": "01k5684kggc5d5mjqpr6rfa5pv",
            "font_size": 100
        },
        {
            "name": "sub_text",
            "order_by": 2,
            "contents_type": 3,
            "contents": null,
            "file_url": null,
            "slide_id": "01k5684kggc5d5mjqpr6rfa5pv",
            "font_size": 100
        },
        .......
    ],
    "isLocked": false,
    "current_step": 5,
    "categories": {
        "01hyacdfj7adef24czzne7wsxa": "G4",
        "01hyacdfjb6n881myy68gr9x4p": "G1",
        "01J1M4S2PS4KH9QR73Q7NC3PA9": "G5",
        "01J1M4S2PS4KH9QR73Q7NC3PAA": "G6",
        "01J1M4S2PS4KH9QR73Q7NC3PAB": "G7",
        "01J1M4S2PS4KH9QR73Q7NC3PAC": "G8",
        "01J1M4S2PS4KH9QR73Q7NC3PAD": "G3",
        "01J1M4S2PS4KH9QR73Q7NC3PAE": "G2"
    },
    "latest_step": 6
}
```

**slide_contents**: Danh sách nội dung slide content sau khi thực hiện undo.

**isLocked**: Trạng thái khóa của slide clip.

**current_step**: Step hiện tại khi undo.

**categories**: Danh sách category của slide.

**latest_step**: Step cuối cùng cho tất cả những thay đổi


### 3. Update slide content

**URI:** `/gmeds/slide-history/update-content`  
**Method:** `POST`

---

### Request Parameters

| Name              | Type        | Required | Description                                 |
|-------------------|-------------|----------|---------------------------------------------|
| `clip_id`         | int/string  | Yes      | ID của slide clip                           |
| `slide_id`        | int/string  | Yes      | ID của slide                                |
| `master_layout_id`| int/string  | Yes      | ID của master layout                        |
| `slide_content`   | object      | Yes      | Thông tin nội dung slide cần cập nhật        |
| `slide_content.name`     | string      | Yes      | Key name của slide content                  |
| `slide_content.contents` | string      | Yes      | Giá trị thay đổi                            |
| `step`            | int         | Yes      | Bước hiện tại                               |

**Response:**
```json
{
    "success": true,
    "current_step": 2,
    "latest_step": 2
}
```

**current_step**: Bước hiện tại sau khi cập nhật.

**latest_step**: Bước lớn nhất hiện tại của slide.