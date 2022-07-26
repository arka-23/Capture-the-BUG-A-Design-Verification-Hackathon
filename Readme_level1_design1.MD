# MUX Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

![image](https://user-images.githubusercontent.com/70422874/180948628-e3b7198e-3dbd-4c59-a4f0-49c08048eb9e.png)

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained. The test drives inputs to the Design Under Test (MUX module here) which takes in 5-bit input *sel* and 31 2-bit inputs *inp* and gives 2-bit output *out*

The values are assigned to the input port using 
```
    dut.inp0.value = 1
    dut.inp1.value = 2
    dut.inp2.value = 1
    dut.inp3.value = 2
    dut.inp4.value = 1
    dut.inp5.value = 2
    dut.inp6.value = 1
    dut.inp7.value = 2
    dut.inp8.value = 1
    dut.inp9.value = 2
    dut.inp10.value = 1
    dut.inp11.value = 2
    dut.inp12.value = 1
    dut.inp13.value = 2
    dut.inp14.value = 1
    dut.inp15.value = 2
    dut.inp16.value = 1
    dut.inp17.value = 2
    dut.inp18.value = 1
    dut.inp19.value = 2
    dut.inp20.value = 1
    dut.inp21.value = 2
    dut.inp22.value = 1
    dut.inp23.value = 2
    dut.inp24.value = 1
    dut.inp25.value = 2
    dut.inp26.value = 1
    dut.inp27.value = 2
    dut.inp28.value = 1
    dut.inp29.value = 2
    dut.inp30.value = 1 

    dut.sel.value = sel
```
![image](https://user-images.githubusercontent.com/70422874/180708014-8b05c385-6e33-4c38-ad23-5c6c0acb76f1.png)

![image](https://user-images.githubusercontent.com/70422874/180708074-08b96270-6b68-46f3-8008-ee19cf9bf928.png)

The assert statement is used for comparing the MUX's outut to the expected value.

## The following error is seen:
```
AssertionError: Randomised test failed with: select line - 01101 corresponding to two different input lines. Possible error in line no. 40
```
![image](https://user-images.githubusercontent.com/70422874/180707564-9f78f528-34da-4af9-a1ef-03be85f639b0.png)

# Test Scenario **(Important)**
- Test Inputs: sel = 01101
- Expected Output: out = 2
- Observed Output in the DUT dut.out = 1

Output mismatches for the above inputs proving that there is a design bug

# Design Bug
Based on the above test input and analysing the design, we see the following

```
 line 40 : 5'b01101: out = inp12;
 line 41 : 5'b01101: out = inp13;
```
For the MUX design, line 40 :  5'b01100: out = inp12, instead of  5'b01101: out = inp12; as in the design code.

## The following error is seen:
```
AssertionError: Randomised test failed with: select - 11110, input not available. Possible error after line no. 57
```
![image](https://user-images.githubusercontent.com/70422874/180707639-66f0b545-57f6-4b52-96d8-aaca66b9554b.png)

# Test Scenario **(Important)**
- Test Inputs: sel = 11110
- Expected Output: out = 1
- Observed Output in the DUT dut.out = 0

Output mismatches for the above inputs proving that there is a design bug

# Design Bug
Based on the above test input and analysing the design, we see the following

```
Input not available. Possible error after line no. 57
```
For the MUX design, line 57:  5'b11110: out = inp30, before default: out = 0; as in the design code.

## The following error is seen:
```
AssertionError: Randomised test failed with: select - 01100, input not available. Possible error after line no. 39
```
![image](https://user-images.githubusercontent.com/70422874/180707446-ded3180a-a2ad-4f8a-b42c-0cf47f65436e.png)

# Test Scenario **(Important)**
- Test Inputs: sel = 01100
- Expected Output: out = 1
- Observed Output in the DUT dut.out = 0

Output mismatches for the above inputs proving that there is a design bug

# Design Bug
Based on the above test input and analysing the design, we see the following

```
Input not available. Possible error after line no. 39
```
For the MUX design, line 40 :  5'b01100: out = inp12, instead of  5'b01101: out = inp12; as in the design code.

## Design Fix
Updating the design and re-running the test makes the test pass.

![image](https://user-images.githubusercontent.com/70422874/180708949-cc5bd573-d6f4-4c10-bfa5-340cb578d8e8.png)


## Verification Strategy

## Is the verification complete ?
