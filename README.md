# DIO_Fintek_F8196x_F8186x

這是一個用於控制 Fintek F81866 晶片 GPIO 功能的 Python 專案。該專案提供了底層硬體存取和 GPIO 控制功能，適用於需要直接控制 GPIO 引腳的嵌入式或工業應用。

## 專案結構

```
DIO_Fintek_F8196x_F8186x/
├── justRead.py          # 主程式：GPIO 狀態監控和變化檢測
├── lib/
│   ├── f81866.py        # F81866 晶片底層存取封裝
│   ├── f81866_gpio.py   # GPIO 控制功能模組
│   └── libf81866.so     # C 語言實作的共享函式庫
└── README.md            # 專案說明文件
```

## 功能特色

- **硬體存取**：透過 I/O 埠直接存取 Fintek F81866 晶片
- **GPIO 控制**：支援 GPIO 輸入/輸出模式設定和狀態讀取
- **即時監控**：持續監控 GPIO 狀態變化並即時回報
- **跨平台**：支援 Linux 系統（需要 root 權限）

## 檔案說明

### justRead.py
主程式檔案，提供以下功能：
- 初始化 F81866 晶片和 GPIO 系統
- 監控 GPIO 0~3 的狀態變化
- 即時顯示 GPIO 狀態變化事件

### lib/f81866.py
F81866 晶片的底層存取封裝，提供：
- `init()`: 初始化晶片
- `unlock()`/`lock()`: 解鎖/鎖定晶片存取
- `read(reg)`: 讀取指定暫存器
- `write(reg, val)`: 寫入指定暫存器
- `set_logic_device(dev)`: 設定邏輯裝置

### lib/f81866_gpio.py
GPIO 控制模組，提供：
- `get_gpio_input(index)`: 讀取指定 GPIO 輸入狀態
- `set_gpio_output(index, value)`: 設定指定 GPIO 輸出狀態
- `set_gpio_direction(index, mode)`: 設定 GPIO 方向（輸入/輸出）
- `init_gpio()`: 初始化 GPIO 系統

### lib/libf81866.so
C 語言實作的共享函式庫，提供底層硬體存取功能。

## 系統需求

- **作業系統**: Linux
- **權限**: 需要 root 權限（sudo）
- **Python**: Python 3.x
- **依賴套件**: ctypes（Python 內建模組）

## 安裝與使用

### 1. 取得 root 權限
```bash
sudo su
```

### 2. 執行程式
```bash
python3 justRead.py
```

### 3. 程式功能
程式會：
1. 初始化 F81866 晶片
2. 設定 GPIO 系統
3. 顯示初始 GPIO 0~3 狀態
4. 持續監控 GPIO 狀態變化
5. 當檢測到狀態變化時，顯示變化詳情

## 使用範例

```python
import lib.f81866 as f81866
import lib.f81866_gpio as gpio

# 初始化
f81866.init()
gpio.init_gpio()

# 讀取 GPIO 0 狀態
value = gpio.get_gpio_input(0)
print(f"GPIO 0: {value}")

# 設定 GPIO 4 為輸出並設為高電平
gpio.set_gpio_direction(4, 1)  # 設為輸出
gpio.set_gpio_output(4, 1)     # 設為高電平
```

## 注意事項

1. **權限要求**: 此程式需要 root 權限才能存取硬體 I/O 埠
2. **硬體相容性**: 僅支援 Fintek F81866 系列晶片
3. **GPIO 範圍**: 支援 GPIO 0~7，其中 GPIO 4~7 預設為輸出模式
4. **即時性**: 程式使用 0.1 秒的輪詢間隔，適合一般應用需求

## 錯誤處理

程式包含以下錯誤檢查：
- I/O 權限檢查
- F81866 晶片存在性檢查
- GPIO 索引範圍檢查

## 授權

此專案為硬體控制專用，請確保在適當的硬體環境中使用。

## 技術支援

如有問題或需要技術支援，請檢查：
1. 硬體連接是否正確
2. 是否具有適當的系統權限
3. F81866 晶片是否正常運作 