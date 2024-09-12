HMC5883 Magnetometer Driver for STM32
This project provides a generic hardware driver for the HMC5883 3-axis digital magnetometer, designed for use with STM32 microcontrollers. 
The driver is implemented using STM32 HAL (Hardware Abstraction Layer) libraries and communicates with the magnetometer via the I2C protocol.

Features
Sample Averaging: Configurable sample averaging options (1, 2, 4, or 8 samples).
Data Output Rate: Configurable data output rate (0.75 Hz to 75 Hz).
Gain Control: Adjustable gain settings for different magnetic field strengths.
Measurement Modes: Supports normal, positive bias, and negative bias modes.
Operating Modes: Continuous measurement, single measurement, and idle modes supported.
I2C Communication: Uses I2C for communication with the HMC5883 sensor.


1. Clone the Repository
Clone the repository to your local machine using the following command:

git clone <https://github.com/mariosoliman6/HMC5883L.git>


2. Include Files in Your Project
Add the HMC5883.h, HMC5883.c, HMC5883_HwInterface.h, and HMC5883_HwInterface.c files to your STM32 project in the appropriate directories (Inc for headers and Src for source files).


3. Configure I2C in STM32CubeMX
Ensure that you have configured the I2C peripheral in STM32CubeMX or directly in your project files.


4. Initialize and Use the HMC5883 Driver
To initialize and use the magnetometer in your main code:


#include "HMC5883.h"

// Declare a HMC5883 structure
HMC5883_t Compass;

int main(void) {
    // Initialize HAL and System
    HAL_Init();
    SystemClock_Config();
    
    // Initialize I2C (done via STM32CubeMX)

    // Initialize HMC5883
    if (HMC5883_Init(&Compass) == HMC5883_OK) {
        printf("HMC5883 Initialized Successfully\n");
    } else {
        printf("HMC5883 Initialization Failed\n");
    }
    
    // Set configurations
    HMC5883_Set_Sample(&Compass, Four_Sample);
    HMC5883_Set_Rate(&Compass, Rate_15HZ);
    HMC5883_Set_Gain(&Compass, Gain_1090_LSBperGauss);
    
    // Main loop
    while (1) {
        // Read magnetometer data
        if (HMC5883_Read(&Compass) == HMC5883_OK) {
            printf("X: %d, Y: %d, Z: %d\n", Compass.X_Output_Data, Compass.Y_Output_Data, Compass.Z_Output_Data);
        } else {
            printf("Failed to read HMC5883 data\n");
        }
        
        HAL_Delay(1000);  // Delay 1 second
    }
}
API Overview
1. HMC5883_Init()
Initializes the HMC5883 with default settings.


HMC5883_Stat_t HMC5883_Init(HMC5883_t *Compass);
2. HMC5883_Set_Sample()
Sets the number of samples to average.


HMC5883_Stat_t HMC5883_Set_Sample(HMC5883_t *Compass, HMC5883_Samples_t Samples);
3. HMC5883_Set_Rate()
Sets the data output rate.


HMC5883_Stat_t HMC5883_Set_Rate(HMC5883_t *Compass, HMC5883_Data_Output_Rate_t Rate);
4. HMC5883_Set_Gain()
Sets the gain for the magnetometer.


HMC5883_Stat_t HMC5883_Set_Gain(HMC5883_t *Compass, HMC5883_Gain_t Gain);
5. HMC5883_Read()
Reads the current magnetometer data (X, Y, Z axis).


HMC5883_Stat_t HMC5883_Read(HMC5883_t *Compass);
Contributions
Contributions are welcome! Feel free to fork this repository and submit pull requests for improvements or new features.

License
This project is licensed under the MIT License. See the LICENSE file for more details.

Contact
For any issues, feel free to create an issue on the GitHub repository or contact the project maintainer at [mariosoliman6@gmail.com].

