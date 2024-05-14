# Example creating and configuration C++ project for STM32

1. Create a clean project using STM32CubeIDE, select MCU/MPU or Board, click `Next`
2. Name the project and select `C++` in the **Target Language** section. Click `Finish`
   
![](img/1.png)

3. In section **Project Manager** -> **Code Generator** select checkbox `Generate peripheral initialization as a pair of '.c/.h' files per peripheral`. After that, all the peripherals of the project will be initialized in separate files, not in `main.c`
![](img/2.png)

4. Set up **Clock Configuration** and **Pinout & Configuration**, generate code after configuration
5. Create an alternative main file. In *Core/Inc* add the file *altMain.hpp*. In *Core/Src* add the file *altMain.cpp*.

6. In *altMain.hpp* paste this code:
   
```cpp
#ifndef INC_ALTMAIN_HPP_
#define INC_ALTMAIN_HPP_


#ifdef __cplusplus
extern "C"
{
#endif
// START C-BLOCK IN C++ CODE

/* Includes ------------------------------------------------------------------*/
#include "usart.h"
// YOUR INCLUDES


/* Functions -----------------------------------------------------------------*/
void setup();
void loop();
// YOUR FUNCTIONS


// END C-BLOCK IN C++ CODE
#ifdef __cplusplus
}
#endif


#endif /* INC_ALTMAIN_HPP_ */
```

IN **YOUR INCLUDES** connect the files we need, but only `C` files. `C++` files are connected in *altMain.cpp*.

In **YOUR FUNCTIONS** we prescribe header of need function. 
I using function `setup()` and `loop()` for writing general code.

7. In *altMain.cpp* prescribe this code:

```cpp
#include "altMain.hpp"
#include "main.h"

void setup() {
    // YOUR SETUP CODE
}

void loop() {
    // YOUR LOOP CODE
}
```

**YOUR SETUP CODE** - code, which will be debugging one time before infinity loop.In this place paste configuration interface, DMA and etc.

**YOUR LOOP CODE** - general code our program.

8. In file *main.c* connect *altMain.hpp* and writing call function `setup()` та `loop()`:

```cpp
// ...
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "altMain.hpp"
/* USER CODE END Includes */
// ...
int main(void)
{
    // ... 
    /* USER CODE BEGIN 2 */
    setup();
    /* USER CODE END 2 */
    
    /* Infinite loop */
    /* USER CODE BEGIN WHILE */
	while (1) {
            loop();
            /* USER CODE END WHILE */

            /* USER CODE BEGIN 3 */
	}
  /* USER CODE END 3 */
}
// ...
```

### Bonus: using class C++ in project

9. If you need create object of class and using her in *altMain*, create file *.hpp* and *.cpp*, and description class:

*classExample.hpp*
```cpp
#ifndef INC_CLASSEXAMPLE_HPP_
#define INC_CLASSEXAMPLE_HPP_

class classExample {
public:
	classExample();
	virtual ~classExample();
};

#endif /* INC_CLASSEXAMPLE_HPP_ */

```

*classExample.cpp*
```cpp
#include <classExample.hpp>

classExample::classExample() {
	// TODO Auto-generated constructor stub

}

classExample::~classExample() {
	// TODO Auto-generated destructor stub
}

```

10. In file *altMain.cpp* connect *classExample.hpp* and creare object of class:
    
```cpp
#include "altMain.hpp"
#include "main.h"
#include "classExample.hpp"

classExample objectExample;

void setup() {
    // YOUR SETUP CODE
}

void loop() {
    // YOUR LOOP CODE
}
```

### WARNING!

Not connecte *.hpp* file in *altMain.hpp*, as for exaple header file our class, so as project will stop building and you get issue 

```cpp
expected '=', ',', ';', 'asm' or '__attribute__' before '{' token	classExample.hpp	/ExampleCppProject/Core/Inc	line 11	C/C++ Problem
``` 
and also 

```cpp
unknown type name 'class'	classExample.hpp	/ExampleCppProject/Core/Inc	line 11	C/C++ Problem
```

**And then we enjoy C++ programming for STM32!**
