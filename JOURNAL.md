# Crossover MMU
Multi-Material Unit with outputs to multiple printers

## Design Constraints and Initial Idea (5/30)
My goal is to create an MMU that has outputs for multiple printers. The main benefit over a traditional mmu is that it allows you to automatically switch filament between printers and function like a regular mmu for each printer. My usecase will mainly be the former, but I might use the mmu functionality for simple/small multicolor prints.

The main functionality I want is:
- A row of input filaments
- A row of outputs that lead to printers
- Ability to swap an output to any available filament 

<img width="624" height="506" alt="image" src="https://github.com/user-attachments/assets/8b91db76-c2aa-4f99-a312-bbfc95a4796c" />

My first idea for this was a system that can pick up small output modules and move them over each other to cross over othe modules to access all avialable filaments. However, I quickly realized the issue. Moving a module over another adds a twist to the ptfe tubes, and these can add up over time.

<img width="471" height="443" alt="image" src="https://github.com/user-attachments/assets/9e28e09f-ccd5-493e-a478-2157c8ffb7fd" />

Now if that left blue output module moved over the orange module, it would add another twist and this cycle could repeat indefinetly if the printers request it.

<img width="440" height="439" alt="image" src="https://github.com/user-attachments/assets/8cb1260b-7dd9-4680-b631-f43fce219f87" />

The only way to prevent this build up of twists in the ptfe tubes is by undoing twists in future moves, which can be done by moving the blue module under the orange.

<img width="462" height="452" alt="image" src="https://github.com/user-attachments/assets/1a8ed78a-23d5-44ff-802d-b607aff1c89d" />

Another realization I had was that I could guarentee the orange and blue module always have 1 or less twists by always moving the blue module under and the orange over during any crossovers. This can scale to any number of outputs, by having a strict ordering of which modules cross over and which ones cross under.

This allows me to essentially give each module its own track and not worry about keeping track of the twists. With 3 modules I could say A always goes over B (B goes under A), and B goes over C, which will ensure twists cancel out over time.

<img width="462" height="210" alt="image" src="https://github.com/user-attachments/assets/74a59b33-6e2b-4f4a-9e9e-54774f4a5fd6" />

Now that I have the twist issue solved I needed to figure out how these modules actually go over and under other modules.
I'd need a secondary manipulator at the bottom inorder to pickup the moving module and move it under the stationary ones, and also have some sort of handoff between the two.

<img width="450" height="285" alt="image" src="https://github.com/user-attachments/assets/40b405f4-2ea1-4134-877e-12d4d79bcf54" />

There's also a couple of other parts to an MMU other than just the selector:
- Filament drive gear
- Moving the drive gear
- Filament Sensor

For the extruder part, im taking inspiration from the ANNEX Tradrack:
<img width="635" height="588" alt="image" src="https://github.com/user-attachments/assets/16b8461d-c499-444e-a903-e0e8ef00d3bb" />
It uses a bmg gearset in the moving carriage, and a bearing as a smooth idler in each filament lane.
Theres a couple of other designs, like the ERCF which uses a main drive axle and press the idler into the filament using the selector, but I think the tradrack solution is the easiest to scale and cheapest per module.

As for moving the drive gear, I'll be moving it vertically linearly, instead of using a servo like the tradrack. This will also serve the purpose of moving the modules up and down.

As for the filament sensor I'm taking inspiration from Tradrack again. Using a small 4mm ball and a switch is simple and reliable. However, I can't put switches in each output module as it would significantly increase the size. So I decided on having the filament sensor attach at the same time as the filament drive gears contact the filament, allowing me to use one switch.

<img width="448" height="395" alt="image" src="https://github.com/user-attachments/assets/b85a6952-6a96-4554-a7cc-5de652684d2b" />

**Hours Spent: 2**

**Total: 2** 

## Output Module & Filament Lane (6/1)
I started with the output module which is pretty simple. Has a ecas coupler for the ptfe tube, and 5 magnets. 2 on top and bottom for the manipulators to move the output module and the last at the rear to attach to the filament lane.
<img width="594" height="517" alt="image" src="https://github.com/user-attachments/assets/6fe0a56a-f604-42de-9a6b-eb9d43698d46" />
<img width="498" height="337" alt="image" src="https://github.com/user-attachments/assets/4a630e99-73e1-4d3a-b325-e68fccc5cf4b" />

The only thing to add to the output module is the nfc tag so it can automatically tell which output is in which filament lane.

Next I worked on the filament lanes.

<img width="660" height="515" alt="image" src="https://github.com/user-attachments/assets/5203184b-451b-4ab4-83d2-d7e3a5a5b155" />

They have an opening for the bmg drive gears to push the filament and a bearing as a smooth idler.

<img width="578" height="448" alt="image" src="https://github.com/user-attachments/assets/86901196-1d12-4722-acb5-a47c72bd4253" />

I also had an opening for the filament sensor to go around the filament.

At the front there's the dock for the output module and a magnet to hold the module in its intermidiate step when switching between the upper manipulator and the lower manipulator.

<img width="476" height="310" alt="image" src="https://github.com/user-attachments/assets/bafd86be-9d35-48a6-b7c6-1a85831a1162" />

But then I realized I needed space for the lower manipulator to slide off of the output module, so I added a gap here. I also added numbers to the filament lanes, and a retaining wall to further prevent modules from falling/moving when filament goes through them.

<img width="702" height="543" alt="image" src="https://github.com/user-attachments/assets/14974046-4128-4127-b795-cd612bd1ee80" />

**Hours Spent: 5**

**Total: 7**

## Extruder/Top carriage (6/18)
The main features of the extruder/top carriage are:
- Reduction and Drive gear
- Filament sensor
- Magnets for moving output modules
- Linear guides
- Belt tensioner/clip

<img width="800" height="562" alt="Screenshot 2026-06-17 232907" src="https://github.com/user-attachments/assets/fb6dabe6-db85-4bc3-baef-5212487b62b2" />

Also I had to figure out how to detect which output module was connected/where each ouput module was.
Originally I was planning on using small rfid/nfc tags, but I realized I could just use the electrical connection through the magnets (similar to klicky probe).
I could use a range of resistors between 1kΩ-10kΩ to differentiate between the output modules.
<img width="947" height="583" alt="image" src="https://github.com/user-attachments/assets/87bca6b0-c4ac-4a0b-904c-8d70571a2ece" />

Heres the wire channel on the carriage:

<img width="702" height="558" alt="image" src="https://github.com/user-attachments/assets/7b3721ef-9602-47dd-995b-7957cc075fa7" />


**Hours Spent: 2**

**Total: 9**

## Bottom carriage and Frame (6/19)
I started with the bottom carriage which just has magnets, belt mounts and linear bearings.

<img width="923" height="698" alt="image" src="https://github.com/user-attachments/assets/27168376-80e1-46db-9ddd-7e5b519fbb68" />

Afterwards I created a parametric frame and added in the components.



<img width="1067" height="624" alt="Screenshot 2026-06-19 003404" src="https://github.com/user-attachments/assets/09c25001-b30f-47c7-bcc7-0abee7399f0e" />

**Hours Spent: 2**

**Total: 9**

## X Motor Mount (6/20)
I wanted to make the mmu compact, so I spent a lot of time trying to fit the motor in between the linear rod mounts. 

<img width="728" height="674" alt="Screenshot 2026-06-19 152156" src="https://github.com/user-attachments/assets/e56ebf1e-c1d0-48f3-a4b0-bf1eb1e6bca9" />

I had to change the carriages to move the belt mounting points.
<img width="949" height="737" alt="image" src="https://github.com/user-attachments/assets/09837719-c072-4116-832d-c744886d63e6" />

<img width="897" height="719" alt="image" src="https://github.com/user-attachments/assets/4fafe9bd-0a84-4f35-b699-4cb262b8ff71" />

I then made the extruder mount.

<img width="462" height="637" alt="image" src="https://github.com/user-attachments/assets/316a1707-2c81-4854-8f94-43da8b00aea1" />
<img width="672" height="619" alt="Screenshot 2026-06-20 002254" src="https://github.com/user-attachments/assets/68c34128-cd5f-47c5-87c9-8cb41f0a978e" />

**Hours Spent: 2**

**Total: 11**

## Z Joints and Z Motor Mounts (6/21)
I decided on using v wheels for the Z axis.

<img width="595" height="589" alt="image" src="https://github.com/user-attachments/assets/369501c6-d681-4364-a6e3-a7497fc4b6c3" />

It's pretty similar to carraiges I've made before so it was pretty straightforward.

I then created the Z motor Mount. It uses a direct drive 16t pulley for simplicity.

<img width="553" height="497" alt="image" src="https://github.com/user-attachments/assets/d15e4fca-9f6c-4750-b29f-f28bb07d8fce" />

Here's how it fits into the full design.

<img width="595" height="593" alt="Screenshot 2026-06-20 235716" src="https://github.com/user-attachments/assets/5223653b-8346-4967-bd2e-e3b963a4fcfe" />

**Hours Spent: 2.5**

**Total: 13.5**

## Design switchup (6/26)
After seeing how large the z joints made the design, I realized I'd much rather use linear rails for Z. 
Also the rods for X would be expensive for anyone else building it and less configurable than linear rail sizes.

So for the redesign I started with the new extruder carriage using an mgn12h carriage.

<img width="833" height="634" alt="Screenshot 2026-06-26 154330" src="https://github.com/user-attachments/assets/42ee0e9f-a823-46f9-8812-b6fcf847f5ac" />

Another significant change I made was "arm" for the magnets that connect to the output modules. 
I realized that ptfe tubes are pretty flexible, even with filament in them. So instead of transferring the output module to a secondary carraige, I could just bend the ptfe tubes a little (8mm) to go under.

This has the significant benefit of removing a rail, belt loop and other hardware.

**Hours Spent: 2**

**Total: 15.5**

## New Frame, X motor Mount, and Z joint (6/27)
Since I only needed one X rail, I could redesign the frame to be a lot more stable.

<img width="941" height="527" alt="image" src="https://github.com/user-attachments/assets/63107cca-981e-44f9-9765-7f61b04be779" />

I then redesigned the X motor mount, including the z idler.

<img width="763" height="667" alt="image" src="https://github.com/user-attachments/assets/8218fd85-ed66-40f7-96e5-26919ad55492" />

I've done a lot of motor mounts like this. It just has a double shear bearing and counterbored screws.

I then designed some simple Z joints.
<img width="737" height="669" alt="image" src="https://github.com/user-attachments/assets/16528807-991a-4a18-bc04-ea93edf5e9cf" />

For both of these designs, I had to adjust them multiple times for clearing other components.

**Hours Spent: 3.5**

**Total: 19**

## Z motor Mounts and Z joints (6/28)
I realized the current z joints will run into the motor mounts and reduce the travel. Also I needed enough space at the top for the z idlers, so I need to maximize space allowed. 

Heres a couple of the designs I tried out before I got to the end result.
<img width="669" height="647" alt="image" src="https://github.com/user-attachments/assets/98d4b457-5b13-4b87-bbba-d2f4da4c8618" />
<img width="656" height="723" alt="image" src="https://github.com/user-attachments/assets/dda20feb-d406-4e34-8706-f64bcb432f2a" />
<img width="787" height="628" alt="image" src="https://github.com/user-attachments/assets/a67b2a52-00d9-443f-99ed-a8e7ecc4267b" />

The final design has a lot of cutouts to clear parts of the z motor mount.

And heres the matching Z motor mount.
<img width="706" height="656" alt="image" src="https://github.com/user-attachments/assets/86ad1421-74a2-4c8a-99d5-314e73adc392" />
<img width="680" height="558" alt="image" src="https://github.com/user-attachments/assets/92bca1f9-0eee-4396-9edb-eaec7376dc3a" />

**Hours Spent: 3**

**Total: 22**

## X redesign and Z idlers (6/29)
This is kindof just an aesthetic redesign since I just didn't like the x axis motor on top of extrusion.
<img width="571" height="693" alt="image" src="https://github.com/user-attachments/assets/a04326c6-f112-4a22-a342-d50211f9f547" />

I moved the tensioner and belt mount on the carriage.
<img width="725" height="631" alt="image" src="https://github.com/user-attachments/assets/6603809c-eae8-4817-8e93-4562de216b95" />

Heres the two part X idler:
<img width="499" height="640" alt="image" src="https://github.com/user-attachments/assets/d8f8e7b7-08c4-4db4-867e-c4cb9c2d2232" />

It's a live idler design since I already have the parts on hand.


I then worked on the Z idler. It needed to clear the extruder motor and fit around the z rail.
<img width="576" height="533" alt="image" src="https://github.com/user-attachments/assets/0fa2086d-7a04-4537-ac95-7a26d7351e24" />

So here's the entire Z axis:
<img width="466" height="772" alt="image" src="https://github.com/user-attachments/assets/1dea307c-300e-49ed-8019-694242df1067" />

**Hours Spent: 4**

**Total: 26**

## Electronics, BOM and Renders (6/30)

I just made some simple mounts for the 24V PSU, mainboard and arduino uno Q (SBC im using):
<img width="923" height="589" alt="image" src="https://github.com/user-attachments/assets/d5bdb3e1-0930-4574-ae69-7e1aa2f21725" />

<img width="725" height="607" alt="image" src="https://github.com/user-attachments/assets/2156ad8f-6687-427e-b9d6-ad0bf4f5234a" />

<img width="830" height="745" alt="image" src="https://github.com/user-attachments/assets/6a5ce157-e1bc-4b06-8ad9-67e44deadb79" />

Heres all of the electronics in the frame:
<img width="1165" height="565" alt="image" src="https://github.com/user-attachments/assets/5ba9bc31-2732-42a3-926b-4e69170161b8" />

I then added ptfe tubes for renders
<img width="1128" height="761" alt="image" src="https://github.com/user-attachments/assets/65149387-53c5-4963-a341-2b66df6b7468" />

<img width="1496" height="842" alt="Crossover_MMU_2026-Jun-30_11-19-06PM-000_CustomizedView32072930422_png_alpha" src="https://github.com/user-attachments/assets/a3da9c0d-5db7-47df-a504-09ef41662013" />
<img width="1239" height="697" alt="Crossover_MMU_2026-Jun-30_11-09-26PM-000_CustomizedView48409742691_png_alpha" src="https://github.com/user-attachments/assets/f935e79d-46c7-4550-a678-dc2f338f90ba" />

The BOM was pretty straightforward since it was easy to find everything on amazon:

<img width="1425" height="709" alt="image" src="https://github.com/user-attachments/assets/7c65eadb-2bcf-46b4-90db-6407c2a67711" />

<img width="1428" height="543" alt="image" src="https://github.com/user-attachments/assets/0ffc0314-8c78-48b7-9286-df5bf7fbb8ce" />

**Hours Spent: 5**

**Total: 31**

