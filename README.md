# Dual-Bank Firmware Update Bootloader for STM32

A **custom bootloader** for STM32 microcontrollers, enabling **safe dual-bank firmware updates** over UART. 
It supports **manual firmware update triggers**, **bank switching**, **CRC verification**, and **host communication protocol handling**.

---

## 🚀 Overview

The bootloader runs at startup and performs the following tasks:
1. **Button check on startup**
   - If the push button is pressed → enters **firmware update / bootloader mode**.
   - If not pressed → jumps to the **currently active firmware bank**.

2. **Firmware update handling**
   - Allows the user to update firmware via UART.
   - Handles command reception, flash erase, memory writes, and verification.
   - Switches the active firmware bank after successful programming.

3. **Dual-bank memory management**
   - **Bank 1 (0x08000000 – 0x0807FFFF)**: Bootloader + Firmware Bank 1  
   - **Bank 2 (0x08080000 – 0x080FFFFF)**: Firmware Bank 2  
   - A small metadata section stores **the active bank number** in the last flash page.

---

## 🧭 Boot Flow

1. **Power-On or Reset**
   - Bootloader initializes peripherals and reads the **active bank number** from flash metadata.

2. **Button Check**
   - **Pressed** → Bootloader Mode (checks for updates).
   - **Not Pressed** → Jumps to user application in the active bank.

3. **Firmware Update Mode**
   - Receives firmware packets from the host (via UART).
   - Performs **CRC validation**, **page erase**, and **flash programming**.
   - Updates the metadata page to mark the newly flashed bank as active.

4. **Normal Operation**
   - On next reboot, jumps directly to the **updated firmware**.

---

## 💾 Memory Map

| Region             | Start Address | Size     | Description |
|--------------------|----------------|-----------|--------------|
| Bootloader         | 0x08000000     | 32 KB     | Custom bootloader |
| Firmware Bank 1    | 0x08008000     | 480 KB    | Primary firmware |
| Firmware Bank 2    | 0x08080000     | 510 KB    | Secondary firmware |
| Metadata Page      | 0x080FF800     | 2 KB      | Active bank info and configuration |

---

## 🔧 Bootloader Commands

| Command | Description |
|----------|-------------|
| `BL_GET_VER` | Get bootloader version |
| `BL_GET_HELP` | Get list of supported commands |
| `BL_GET_CID` | Get MCU chip ID |
| `BL_GET_RDP_STATUS` | Read protection status |
| `BL_FLASH_ERASE` | Erase flash sectors |
| `BL_MEM_WRITE` | Write memory |
| `BL_SHOW_ACTIVE_BANK` | Display current active bank |

All commands follow a **length-prefixed UART packet structure** with **CRC32 validation** for data integrity.

---

## 🧩 Host Communication

- The bootloader communicates with a **Python-based host application**.
- Host sends command packets in a predefined structure.
- Supports **chunk writing** of `.bin` firmware files.
- Acknowledgment and error feedback are provided through UART logs.

---

## ⚙️ Key Functions

- **`bootloader_uart_read_data()`**  
  Handles UART command reception and dispatch.

- **`bootloader_jump_to_active_bank()`**  
  Sets MSP, remaps vector table, and jumps to firmware Reset_Handler.

- **`bootloader_handle_mem_write_cmd()`**  
  Handles memory programming with alignment and verification.

- **`fetch_active_bank_number()` / `update_active_bank_number()`**  
  Reads or updates metadata flash page to store active firmware info.

- **`handle_firmware_update()`**  
  Performs manual firmware update sequence via host communication.

---

## 🛠️ Hardware Used

- **STM32L4 series MCU** (tested on Nucleo Board)
- **Two UART interfaces:**
  - Debug UART (LPUART1)
  - Command UART (USART2)
- **Push button** for entering bootloader mode
- **LEDs (optional)** for status indication

---

## 🧠 Key Features

- ✅ Dual-bank firmware management  
- ✅ UART-based firmware updates  
- ✅ Metadata-driven bank switching  
- ✅ CRC-based integrity verification  
- ✅ Debug UART messages
