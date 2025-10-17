/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.h
  * @brief          : Header for main.c file.
  *                   This file contains the common defines of the application.
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2025 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Define to prevent recursive inclusion -------------------------------------*/
#ifndef __MAIN_H
#define __MAIN_H

#ifdef __cplusplus
extern "C" {
#endif

/* Includes ------------------------------------------------------------------*/
#include "stm32l4xx_hal.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include <stdarg.h>
#include <stdint.h>
#include <string.h>
#include <stdio.h>

/* USER CODE END Includes */

/* Exported types ------------------------------------------------------------*/
/* USER CODE BEGIN ET */

/* USER CODE END ET */

/* Exported constants --------------------------------------------------------*/
/* USER CODE BEGIN EC */

/* USER CODE END EC */

/* Exported macro ------------------------------------------------------------*/
/* USER CODE BEGIN EM */

/* USER CODE END EM */

/* Exported functions prototypes ---------------------------------------------*/
void Error_Handler(void);

/* USER CODE BEGIN EFP */

/*Boot loader function prototypes*/
void bootloader_uart_read_data(void);
void bootloader_jump_to_active_bank(void);

/*Boot loader command handling functions*/
void bootloader_handle_getver_cmd(uint8_t *bl_rx_buffer);
void bootloader_handle_gethelp_cmd(uint8_t *pBuffer);
void bootloader_handle_getcid_cmd(uint8_t *pBuffer);
void bootloader_handle_getrdp_cmd(uint8_t *pBuffer);
void bootloader_handle_go_cmd(uint8_t *pBuffer);
void bootloader_handle_mem_write_cmd(uint8_t *pBuffer);
void bootloader_handle_flash_erase_cmd(uint8_t *pBuffer);
void bootloader_handle_en_rw_protect(uint8_t *pBuffer);
void bootloader_handle_dis_rw_protect(uint8_t *pBuffer);
void bootloader_show_active_bank(void); /*Function of the boot loader returns active bank number */

/*Boot loader helper functions*/
void printmsg(char *format, ...);
void bootloader_send_ack(uint8_t follow_len);
void bootloader_send_nack(void);
uint8_t bootloader_verify_crc (uint8_t *pData, uint32_t len,uint32_t crc_host);
void bootloader_uart_write_data(uint8_t *pBuffer,uint32_t len);
uint8_t get_bootloader_version(void);
uint16_t get_mcu_chip_id(void);
uint8_t get_flash_rdp_level(void);
uint8_t verify_address(uint32_t go_address);
uint8_t execute_flash_erase(uint8_t page_number , uint8_t number_of_pages);
uint8_t execute_mem_write(uint8_t *pBuffer, uint32_t mem_address, uint32_t len);

/*Firmware update and related functions*/
uint8_t fetch_available_firmware_version(void);
void handle_firmware_update(void);
uint8_t fetch_active_bank_number(void); /*Updates the global variable with the fetched active bank value from the FLASH*/
void update_active_bank_number(uint8_t active_bank);



/* USER CODE END EFP */

/* Private defines -----------------------------------------------------------*/
#define B1_Pin GPIO_PIN_13
#define B1_GPIO_Port GPIOC
#define MCO_Pin GPIO_PIN_0
#define MCO_GPIO_Port GPIOH
#define LD3_Pin GPIO_PIN_14
#define LD3_GPIO_Port GPIOB
#define USB_OverCurrent_Pin GPIO_PIN_5
#define USB_OverCurrent_GPIO_Port GPIOG
#define USB_PowerSwitchOn_Pin GPIO_PIN_6
#define USB_PowerSwitchOn_GPIO_Port GPIOG
#define STLK_RX_Pin GPIO_PIN_7
#define STLK_RX_GPIO_Port GPIOG
#define STLK_TX_Pin GPIO_PIN_8
#define STLK_TX_GPIO_Port GPIOG
#define USB_SOF_Pin GPIO_PIN_8
#define USB_SOF_GPIO_Port GPIOA
#define USB_VBUS_Pin GPIO_PIN_9
#define USB_VBUS_GPIO_Port GPIOA
#define USB_ID_Pin GPIO_PIN_10
#define USB_ID_GPIO_Port GPIOA
#define USB_DM_Pin GPIO_PIN_11
#define USB_DM_GPIO_Port GPIOA
#define USB_DP_Pin GPIO_PIN_12
#define USB_DP_GPIO_Port GPIOA
#define TMS_Pin GPIO_PIN_13
#define TMS_GPIO_Port GPIOA
#define TCK_Pin GPIO_PIN_14
#define TCK_GPIO_Port GPIOA
#define SMPS_V1_Pin GPIO_PIN_10
#define SMPS_V1_GPIO_Port GPIOG
#define SMPS_EN_Pin GPIO_PIN_11
#define SMPS_EN_GPIO_Port GPIOG
#define SMPS_PG_Pin GPIO_PIN_12
#define SMPS_PG_GPIO_Port GPIOG
#define SMPS_SW_Pin GPIO_PIN_13
#define SMPS_SW_GPIO_Port GPIOG
#define SWO_Pin GPIO_PIN_3
#define SWO_GPIO_Port GPIOB
#define LD2_Pin GPIO_PIN_7
#define LD2_GPIO_Port GPIOB

/* USER CODE BEGIN Private defines */

//version 1.0
#define BL_VERSION 0x10

// Custom boot loader commands

//#define  <command name >  <command_code>

//This command is used to read the bootloader version from the MCU
#define BL_GET_VER        		0x51

//This command is used to know what are the commands supported by the bootloader
#define BL_GET_HELP      	  	0x52

//This command is used to read the MCU chip identification number
#define BL_GET_CID        		0x53

//This command is used to read the FLASH Read Protection level.
#define BL_GET_RDP_STATUS  		0x54

//This command is used to jump bootloader to specified address.
#define BL_GO_TO_ADDR     		0x55

//This command is used to mass erase or sector erase of the user flash .
#define BL_FLASH_ERASE          0x56

//This command is used to write data in to different memories of the MCU
#define BL_MEM_WRITE      		0x57

//This command is used to enable or disable read/write protect on different sectors of the user flash .
#define BL_EN_RW_PROTECT    	0x58

//This command is used to read all the sector protection status.
#define BL_READ_SECTOR_P_STATUS 0x5A

//This command is used disable all sector read/write protection
#define BL_DIS_R_W_PROTECT      0x5C

//Firmware and update related commands
#define BL_SHOW_ACTIVE_BANK		0x60


/* ACK and NACK bytes*/
#define BL_ACK   				0XA5
#define BL_NACK 				0X7F

/*CRC*/
#define VERIFY_CRC_FAIL    		1
#define VERIFY_CRC_SUCCESS 		0

#define ADDR_VALID 				0x00
#define ADDR_INVALID 			0x01

#define INVALID_SECTOR 			0x04


#define SRAM1_SIZE            256*1024
#define SRAM1_END             (SRAM1_BASE + SRAM1_SIZE)
#define SRAM2_END             (SRAM2_BASE + SRAM2_SIZE)


/* USER CODE END Private defines */

#ifdef __cplusplus
}
#endif

#endif /* __MAIN_H */
