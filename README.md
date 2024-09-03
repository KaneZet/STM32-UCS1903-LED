# STM32-UCS1903-LED
CubeIde project for UCS1903 LED on STM32F103C8T6
# Настройка проекта в CubeMX:
1. Pinout & Configuration -> Timers -> TIM1
Clock Source - Internal clock
Channel 1 - PWM Generation CH1
2. DMA Settings -> Add -> TIM1_CH1
Direction - Memory to Peripheral
Mode - circular
Increment Address - o v
Width - word byte
3. GPIO settings
Maximum output speed - high
4. Clock configuration -> HCLK > 32 MHz
# Настройка ARGB.h
#define WS2812 - поменять на WS2811S
#define TIM_NUM - номер таймера
#define TIM_CH	   TIM_CHANNEL_X - X это номер канала
#define DMA_HANDLE - вставить DMA_HandleTypeDef из main.c
#define DMA_SIZE_WORD - поменять на #define DMA_SIZE_BYTE
# Настройка ARGB.c
151| PWM_HI = (u8_t) (APBfq * 0.48) - 1; - 0.48 поменять на 0.80
# Функции
void ARGB_Init(void);   // Инициализация
void ARGB_Clear(void);  // Очистить ленту

void ARGB_SetBrightness(u8_t br); // Установить глобальную яркость

void ARGB_SetRGB(u16_t i, u8_t r, u8_t g, u8_t b);  // Установить RGB цвет 1 пикселя
void ARGB_SetHSV(u16_t i, u8_t hue, u8_t sat, u8_t val); // Установить HSV цвет 1 пикселя

void ARGB_FillRGB(u8_t r, u8_t g, u8_t b); // Залить ленту RGB цветом
void ARGB_FillHSV(u8_t hue, u8_t sat, u8_t val); // Залить ленту HSV цветом

ARGB_STATE ARGB_Ready(void); // Получить состояние готовности DMA
ARGB_STATE ARGB_Show(void); // Отправить данные в ленту
# Примеры
ARGB_Init();
    
ARGB_Clear();
while (ARGB_Show() == ARGB_BUSY) ; // Вариант 1

ARGB_SetRGB(0, 255, 0, 128);
ARGB_SetHSV(1, 230, 250, 255);
while (!ARGB_Show()) ; // Вариант 2

ARGB_SetRGB(3, 200, 0, 200);
// Вариант 3:
while (ARGB_GetState() != ARGB_READY) ;
ARGB_Show();
