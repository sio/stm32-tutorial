# Осваиваем STM32 снизу

В данной статье мы попробуем поработать с процессором STM32 с помощью GNU
утилит, немного познакомимся с ассемблером и отладкой.

Примеры написаны для популярной платы blue pill, построенной на микроконтроллере
STM32F103C8T6.

Примерный план:

1. Подключить плату к компьютеру и убедиться, что там что-то происходит.
   Использовать будем st-util и gdb.

2. Написать простейшую программу на ассемблере, которая в цикле прибавляет
   регистр, скомпилировать из неё прошивку, залить на плату и пронаблюдать её
   работу. Использовать будем binutils и st-flash.

3. Поморгать диодом (на ассемблере же).

4. Переписать осмысленный код на С (дальше всё на С).

5. Переписать моргание с использованием таймера, чтобы внести свой вклад в
   борьбу с глобальным потеплением.

6. Сказать внешнему миру "Hello world" через UART.

7. Переписать "Hello world" с помощью CMSIS, уже с пониманием того, что там
   происходит.

В процессе будет использовано достаточно много инструментов вроде make, ld, gdb,
as, gcc и тд, по каждому из них можно книги писать (и пишут). Поэтому, конечно,
углубляться в них мы не будем, а напротив, эти инструменты будут использоваться
в максимально примитивном виде.

Ниже ссылки на документацию, где можно подробней разобраться с любыми нюансами.

- [STM32F103xx Reference Manual](https://www.st.com/resource/en/reference_manual/rm0008-stm32f101xx-stm32f102xx-stm32f103xx-stm32f105xx-and-stm32f107xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf).
  Тут вся справочная информация по всем битам и байтам данного микроконтроллера.
- [STM32F10xxx Programming manual](https://www.st.com/resource/en/programming_manual/pm0056-stm32f10xxx20xxx21xxxl1xxxx-cortexm3-programming-manual-stmicroelectronics.pdf).
  Тут справочная информация по ARM: все инструкции, описание работы процессора.
- [GNU debugger GDB](https://sourceware.org/gdb/current/onlinedocs/gdb.html/).
  Это отладчик.
- [GNU make](https://www.gnu.org/software/make/manual/html_node/index.html). Это
  утилита для сборки проектов.
- [GNU linker ld](https://sourceware.org/binutils/docs/ld/). Это компоновщик
  (линкер).
- [GNU assembler as](https://sourceware.org/binutils/docs/as/). Это ассмблер.
