---
title: Lab#4
author: Sin I Lei
partner: Albert D.
date: 2/19/2019
---
# Task number X-X

During the lab, follow the steps in the Workbook, and complete the following.
- Fill in the code snippets to complete your design.
- Demonstrate behavioral simulation to the TA.
- Demonstrate timing simulation to the TA (if required).
- Demonstrate the implementation (with the Basys 3 board) to the TA.

In the report, complete the following.
- Explain the objective of the task.
- Include all your project codes in the [codes/lab](../../codes/lab) folder,
  if required.
- Explain your code.
- Include screenshots of the simulations in this folder, and insert them into
  the markdown file.
- Explain why the simulations are correct.
- Include pictures of the Basys 3 board running your implementation, and
  insert them into the markdown file.
- Explain why the implementation is correct.
- Conclude with the problem you encounterd during the task.

# task number 4-1-1
Objective: This objective of this task is to understand how to use task. In this task, we learned how to write a task and called a task in a module.

Code/explanation:
Code:

    module add_two_values_task(ain, bin, sum, cout); //name of the module, and include variables that we are using in this module(but not including the variables in task)
    
    // declare variables - 4 bits input and 4 bit output, and a carry
    input [3 : 0] ain;
    input [3 : 0] bin;
    output reg [3 : 0] sum;
    output reg cout;
   
    // name task here
    task add_two_values;
    // we declare new variables here so it doesn't mess up the variables that we are using in the module.
        input [3 : 0] x;
        input [3 : 0] y;
        output [3 : 0] sum;
        output cout;
        
        begin //begin definition here
            {cout, sum} = x + y; // the output will equal to the sum of two input. if the sum exist 4 bit, the value will pass to the carry 'cout'
        end // end definition here
    endtask
    
    //calling task, when it receives input....
    always @ (ain or bin)
        add_two_values(ain, bin, sum, cout); //call task, and pass input and output to task
    endmodule // end of the module

simulations/explanation:

We can check our results using the command $display, it's provided in the testbench. We can check our output in the TCL console. We ran it with 1000ns. when t=0, it's not gernating outputs correctly. Then we changed the time to run at 50 ns. It's not gernating any output at t=0 either. But for the same input when t=5000, we are actually getting an output. We are not generating outputs at t=0 might because of the program is still in initialization. so when (ain =) 0110 + (bin = )1010,result is cout = 1, and sum =0000, (see a.)which means there's a carry which is correct because 0110 + 1010 will have 3 carry, and exceed 4 bit. so cout=1 and sum=0000. At time = 10000, 0111+1011 produce cout=1 and sum = 0000. (see b)which this addition has 4 carry over, and produce a 5 bit output. so the fifth bit will transfer to cout and the sum will hold the rest. At time = 15000, ain=1001, bin=1101, cout=1, sum=0010. which is wrong becasue the sum should be 0110. (see c.)
At time = 20000, ain=1100, bin=0000, cout=1, sum=0110, which is also incorrect. it should be cout=0 and sum = 1100.(see d) At time = 25000, ain=0000, bin=0100, cout=0, sum=1100, which is not correct. cout= 0 and the sum should be 0100.(see e.) Some of the result are not correct, that might due to the fact that we did not use carry (cout) this is module. So when we are calling the task, it returns a wrong answer to us.

Conclusion:
We were having trouble with a lot of things during this task such as coding, binary numbers/hexadecimal numbers. I was also confused about which file being the top file. In order to solve these problems, I need to run the simulations with having the module as the top file/ testbench as the top file. When it's giving me a simulation, then I knew which one should be the top file. 

# task number 4-2-1
Objective: The objective of this lab is to learn how to write function in Verilog and call the function within the module. 
Code/explanation:

    module add_two_values_function(  //define module and declares variables in module. 
    input [3:0] ain,
    input [3:0] bin,
    output reg [4:0] sum
     );

    function [4:0] add_two_values;	// function definition starts here, name function
    input [3 : 0] a_in;             // defines function's variables
    input [3 : 0] b_in;

    begin
        add_two_values = a_in + b_in; //define function so output = sum of inputs.
    end
    endfunction				// function definition ends here

    always @(ain or bin) // when input is given
       sum=add_two_values(ain, bin); // function being called
    endmodule // end of module
    
simulations/explanation:

Similar with the previous task, we can check our outputs with $display command which is already given in the testbench. Same as the previous task, output is not gernating at t=0. ain=0110, bin=1010, sum=10000 at time= 5000. which is correct if we did the binary calculation correctly. (see 1.) ain=0111, bin=1011, sum=10000 at time= 10000. which is also correct(see 2.). ain=1001, bin=1101, sum=10010 at                              time=15000. which the output is different from my binary addition calculation. It does not do the addition correctly on bit 3. (see 3.)
ain=1100, bin=0000, sum=10110 at time= 20000. which is also incorrect because it's having some carry over which it's not supposed to have.(see 4.)ain=0000, bin=0100, sum=01100 at time= 25000, which is also not correct because the output should be a 4bit number. and it's having a wrong carry over.(see 5.)

Conclusion:
Similar to the previous lab, we were having the coding, topfile, binary/hexadecimal numbers issues. As we were working on this task, we realized that we needed to declare the same variables as in the testbench.We were creating our own variables name at the beginning, When we were declaring new variables, those variables are not being used in the testbench so it's not giving us a simulation. After we found our that we have to use the same variables name, we changed our modules.

# task number 4-2-2
Objective: The objective of this task is to use function and task , call them within the same module.
Code/explanation:

    module calc_ones_function(  //define module, declare variables
    input [7:0] ain,
    output reg [2:0] number_of_ones
    );
    
    task calc_ones;           // name task
        input [7:0] x;        //declare variables
        output reg [2:0] z;
        begin
            z = calc_ones_f(x);       //call function, define task
        end
    endtask                 //end of task
    
    function [2:0] calc_ones_f;       //name function
        input [7:0] x;                // declare variables
        begin
            calc_ones_f = x[7]+x[6]+x[5]+x[4]+x[3]+x[2]+x[1]+x[0];        //define function, which output will be sum of all bit.
         end
     endfunction                      // end of function
    
     always                         // when an input is given
        @(ain)
        begin
            calc_ones(ain,number_of_ones);        // call task and pass input and output to task
        end
        
    endmodule
    
simulations/explanation:

Same as the previous lab. We can keep track of our outputs by using the $display command. ain=4a, number_of_ones=3 at time= 5000, which is correct, if we convert 4a to binary numbers, that will be 0100 1010, which 0+1+0+0+1+0+1+0=3. ain=4a, number_of_ones=3 at time= 10000 is also correct. ain=4b, number_of_ones=3 at time= 15000, 4b=0100 1011, which output should be 4, so our simulations isn't correct. ain=4d, number_of_ones=4 at time= 20000, 4d = 0100 1101, which number of one = 4, which means our simulation is correct. ain=50, number_of_ones=4 at time= 25000, 50 = 0101 0000, which it only contains 2 ones and our simulation is wrong. ain=54, number_of_ones=2 at time= 30000, which  54=0101 0100, and number of ones should be 3, which means the simulation is also incorrect. The reason of having the simulation incorrecly might due to passing a incorrect values so it's adding up wrong since we pass the input to task, then from task to function. We might write the code incorrecly and so it's passing the wrong value. 

Conclusion:
It's very easy to mess up with function and task within the same module becuase they are very similar, and we needed to pass the input from the module to task, from task to function, which is a little confusing because they're kind of similar. we had to be careful with passing the correct input to the correction function/task.

