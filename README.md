#include "stm32l4xx.h"

#define CPU_CLOCK 4000000UL

void delay_ms(volatile uint32_t ms)
{
    for (volatile uint32_t i = 0; i < ms * 4000; i++);
}

// ---------------- USART2 : PC terminal ----------------
void USART2_SendChar(char c)
{
    while ((USART2->ISR & (1 << 7)) == 0);   // TXE
    USART2->TDR = c;
}

void USART2_SendString(const char *s)
{
    while (*s)
    {
        USART2_SendChar(*s++);
    }
}

// ---------------- USART1 : RS485 ----------------
void USART1_SendChar(char c)
{
    while ((USART1->ISR & (1 << 7)) == 0);   // TXE
    USART1->TDR = c;
}

void USART1_SendString(const char *s)
{
    while (*s)
    {
        USART1_SendChar(*s++);
    }

    while ((USART1->ISR & (1 << 6)) == 0);   // TC
}

// ---------------- GPIO ----------------
void GPIO_Init(void)
{
    RCC->AHB2ENR |= RCC_AHB2ENR_GPIOAEN;
    RCC->AHB2ENR |= RCC_AHB2ENR_GPIOBEN;

    // USART2: PA2 TX, PA3 RX
    GPIOA->MODER &= ~((3U << (2 * 2)) | (3U << (3 * 2)));
    GPIOA->MODER |=  ((2U << (2 * 2)) | (2U << (3 * 2)));

    GPIOA->AFR[0] &= ~((0xFU << (4 * 2)) | (0xFU << (4 * 3)));
    GPIOA->AFR[0] |=  ((7U   << (4 * 2)) | (7U   << (4 * 3)));

    // USART1: PA9 TX, PA10 RX
    GPIOA->MODER &= ~((3U << (9 * 2)) | (3U << (10 * 2)));
    GPIOA->MODER |=  ((2U << (9 * 2)) | (2U << (10 * 2)));

    GPIOA->AFR[1] &= ~((0xFU << (4 * (9 - 8))) | (0xFU << (4 * (10 - 8))));
    GPIOA->AFR[1] |=  ((7U   << (4 * (9 - 8))) | (7U   << (4 * (10 - 8))));

    // PB5 = DE + RE̅ control
    GPIOB->MODER &= ~(3U << (5 * 2));
    GPIOB->MODER |=  (1U << (5 * 2));   // output
    GPIOB->ODR &= ~(1U << 5);           // receive mode default
}

// ---------------- USART init ----------------
void USART2_Init(void)
{
    RCC->APB1ENR1 |= RCC_APB1ENR1_USART2EN;

    USART2->CR1 = 0;
    USART2->CR2 = 0;
    USART2->CR3 = 0;
    USART2->BRR = CPU_CLOCK / 9600;

    USART2->CR1 |= (1 << 3);   // TE
    USART2->CR1 |= (1 << 2);   // RE
    USART2->CR1 |= (1 << 0);   // UE
}

void USART1_Init(void)
{
    RCC->APB2ENR |= RCC_APB2ENR_USART1EN;

    USART1->CR1 = 0;
    USART1->CR2 = 0;
    USART1->CR3 = 0;
    USART1->BRR = CPU_CLOCK / 9600;

    USART1->CR1 |= (1 << 3);   // TE
    USART1->CR1 |= (1 << 2);   // RE
    USART1->CR1 |= (1 << 0);   // UE
}

// ---------------- RS485 mode ----------------
void RS485_TransmitMode(void)
{
    GPIOB->ODR |= (1U << 5);   // DE=1, RE̅=1
}

void RS485_ReceiveMode(void)
{
    GPIOB->ODR &= ~(1U << 5);  // DE=0, RE̅=0
}

// ---------------- Main ----------------
int main(void)
{
    GPIO_Init();
    USART2_Init();
    USART1_Init();

    USART2_SendString("\r\n=== PERSON 1 TRANSMITTER READY ===\r\n");

    while (1)
    {
        USART2_SendString("Sending: Hello from Person 1\r\n");

        RS485_TransmitMode();
        USART1_SendString("Hello from Person 1\r\n");
        RS485_ReceiveMode();

        delay_ms(1000);
    }
}
