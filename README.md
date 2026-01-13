# Mini-Greenhuse - AgroTech final project
This project monitors and controls VPD in a small greenhouse. Temperature and humidity are measured to calculate VPD, which drives a fan or mist to optimize conditions.

## Introduction
In our final project, we developed a mini-greenhouse equipped with temperature, humidity, and leaf wetness sensors. These sensors continuously monitor both indoor and outdoor conditions.
Using the collected data, we calculate the Vapor Pressure Deficit (VPD) to assess the plant environment.
Our model plant is tomato, and based on its optimal growth conditions, the system controls humidity using a fan or mist sprayer to maintain ideal growing conditions.

### Background
VPD, or Vapor Pressure Deficit, is the difference between the amount of moisture in the air and the maximum amount of moisture the air can hold when saturated.
In plant physiology, VPD helps us understand how environmental conditions affect a plant’s ability to grow and produce fruit, by influencing transpiration. 
Transpiration is the movement of water from the roots through the xylem into the leaf mesophyll, followed by evaporation from the leaf into the atmosphere.

**The effects of VPD on plants:**

| VPD Condition       | Effect on Plant |
|--------------------|----------------|
| **High VPD**        | High transpiration → excessive water loss → stomata close → reduced photosynthesis |
| **Low VPD**         | Low transpiration → reduced water and nutrient transport → slower growth |

Therefore, VPD has a major impact on plant growth and fruit production, making it a critical parameter to monitor and control in greenhouse environments.


<img width="200" height="676" alt="image" src="https://github.com/user-attachments/assets/5fc37e92-b156-4970-aa00-13122fd66a40" />

* Stomatal conductance is the rate of gas exchange between the leaf and the atmosphere through the stomata, regulating transpiration and CO₂ uptake for photosynthesis.

## Hardware and assembly

1. **FireBeetle 2 ESP32-E**  
   - Microcontroller for sensor reading and actuator control  
   <img src="https://github.com/user-attachments/assets/788be901-a0e3-4e1b-beda-a9640001b892" width="150" />

2. **SHT31 Temperature & Humidity Sensor**  
   - One sensor inside the greenhouse  
   - One sensor outside for reference and fan control  
<img src="https://github.com/user-attachments/assets/7d1fa548-46a7-4f3a-b27e-206cbe59a1ba" width="150" />

3. **Leaf Wetness Sensor**  
   - Simulates a tomato leaf to measure surface moisture  
<img src="https://github.com/user-attachments/assets/8ebb374a-412e-4a11-8bb6-9abcf2a380d0" width="150" />

4. **Fan**  
   - DC fan (5–12V depending on model) for ventilation and VPD control  
   - Controlled via **relay**  

5. **Mist Sprayer**  
   - Activates only if **VPD is high** and leaf is dry  

6. **Buck Converter**  
   - Converts 12V supply to 5V for ESP32 and sensors  
<img src="https://github.com/user-attachments/assets/6b067c58-f87a-41ff-b429-d868455b7664" width="150" />

7. **Relay Module**  
   - Controls Fan and Mist power circuits

### General Structure

The greenhouse is a **plastic box** creating a controlled microclimate.  

- **Side ventilation** allows outside air (via fan) to enter when needed.  
- **Inside**:  
  - SHT31 sensor for indoor VPD  
  - Leaf wetness sensor  
  - Mist sprayer  

The system monitors both indoor and outdoor conditions to decide whether to **ventilate** or **spray mist**.


### Data Collected

1. **Temperature & Humidity** (via SHT31)  
2. **VPD** (calculated from temperature & humidity)  
3. **Leaf Wetness** (via analog sensor)  
4. **Plant Growth Status** (based on VPD range: optimal → +1, suboptimal → -1)  


### VPD Control Logic

The system controls the environment using the following logic:

| Condition                          | Action |
|-----------------------------------|--------|
| **VPD too low (indoor)**           | Fan ON → increase VPD via airflow |
| **VPD too high + leaf dry**        | Mist ON → increase leaf moisture, reduce VPD |
| **VPD too high + leaf wet**        | Do nothing (leaf already moist) |
| **VPD optimal**                     | All actuators OFF |
