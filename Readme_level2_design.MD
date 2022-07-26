# Challenges-arka-23
Challenges-arka-23 created by GitHub Classroom
# Bitmanipulation Coprocessor Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

![image](https://user-images.githubusercontent.com/70422874/180929690-d6604e02-f38c-4fe6-9dc7-b5f2b4bb003a.png)

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained. The test drives inputs to the Design Under Test (Sequence Detector module here) which takes in 1-bit inputs *inp_bit* as a sequence synchronized to a clock, a 1-bit *reset* and gives 1-bit output *seq_seen*

The values are assigned to the input port using 
```
      for _ in range(50):
        # reset
        dut.RST_N.value <= 0
        yield Timer(10) 
        dut.RST_N.value <= 1

        ######### CTB : Modify the test to expose the bug #############
        # input transaction
        mav_putvalue_src1 = random.randint(0, 15)
        mav_putvalue_src2 = random.randint(0, 15)
        mav_putvalue_src3 = random.randint(0, 15)
        mav_putvalue_instr = 0x110150B3

        # expected output from the model
        expected_mav_putvalue = bitmanip(mav_putvalue_instr, mav_putvalue_src1, mav_putvalue_src2, mav_putvalue_src3)

        # driving the input transaction
        dut.mav_putvalue_src1.value = mav_putvalue_src1
        dut.mav_putvalue_src2.value = mav_putvalue_src2
        dut.mav_putvalue_src3.value = mav_putvalue_src3
        dut.EN_mav_putvalue.value = 1
        dut.mav_putvalue_instr.value = mav_putvalue_instr
    
```

![image](https://user-images.githubusercontent.com/70422874/180929781-81a9b5e3-e3ef-4506-a205-3ddf8ebcc700.png)


The assert statement is used for comparing the MUX's outut to the expected value.

## The following error is seen:
```
 AssertionError: Value mismatch DUT = 0x16 does not match MODEL = 0x0. Instruction not present, expected output is 0x0
 ```
![image](https://user-images.githubusercontent.com/70422874/180930057-6e7a6469-9fc9-4520-b6bf-57fd1c9fbd8f.png)
 
## Test Scenario **(Important)**
- mav_putvalue_src1 = random.randint(0, 15)
- mav_putvalue_src2 = random.randint(0, 15)
- mav_putvalue_src3 = random.randint(0, 15)
- mav_putvalue_instr = 0x110150B3
- Expected Output: expected_mav_putvalue = 0x0
- Observed Output in the DUT: dut_output = 0x16

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
Instruction not present, expected output is 0x0
```

## Design Fix
Updating the design and re-running the test makes the test pass.


## Verification Strategy

## Is the verification complete ?
