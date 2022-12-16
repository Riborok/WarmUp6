# WarmUp №6. Long arithmetic (product)
---
### Task:
![The task](https://i.imgur.com/NuAldiZ.png)

>The program calculates the product of the numbers.

#### Language: Delphi

### Code:
``` pascal
Program WarmUp6;
{
 The program should calculate the product of n numbers in the entered number system
}

{$APPTYPE CONSOLE}

Uses
  System.SysUtils;

Const
  MaxSize=256;
  MaxAmount=50;
  NSAlphabet = '0123456789ABCDEFGHIJ';
  MaxNS = length(NSAlphabet);
  //MaxSize - maximum amount of digits for entered numbers
  //MaxAmount - maximum amount of numbers
  //NSAlphabet - transfer between symbols and numbers
  //MaxNS - maximum number system

Var
  Num1 :array[1..(MaxSize * MaxAmount)] of SmallInt;
  Num2 :array[1..MaxSize] of SmallInt;
  ProdNums :array[1..(MaxSize * MaxAmount)] of SmallInt;
  str :string;
  NS, Amount : Integer;
  j, Sum, CarrySum : ShortInt;
  Len1, Len2, i, k, Prod, CarryProd, PosElement : Word;
  flag: boolean;
  //Num1 - array of digits of the first number (further the product of the two previous numbers)
  //Num2 - array of digits of the second numbers (with which need to multiply the first number)
  //ProdNums - array of digits of product of numbers (in the process of multiplication)
  //str - string variable for writing numbers
  //NS - number system
  //Amount - amount of numbers
  //Sum - the sum of all values in the current element (for ProdNums)
  //CarrySum - сarry 1 (if there is) to the next element (for Sum)
  //Len1 - the length of the first number
  //Len2 - the length of the second number
  //Prod - product of the digits of the first and second number
  //CarryProd - сarry the digit (if there is) to the next element (for Prod)
  //PosElement - the current position of the element in the multiplication
  //i, j, k - cycle counter
  //flag - flag to confirm the correctness of entering numbers


Begin

  Writeln('Enter number system (maximum number system is ',MaxNS,', minimal is 2). The program will calculate their product.');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Validating the correct input data type
    Try
      Readln(NS);
    Except
      Writeln('Wrong input of number system! It must be an integer');
      flag:= True;
    End;

    //Validate Range
    if ((NS > MaxNS) or (NS < 2)) and (flag = False) then
    begin
      Writeln('Wrong input of number system! It must be >=2 and <=',MaxNS);
      flag:= True;
    end;

  Until flag = False;

  Writeln;
  Writeln('Enter amount of numbers (no more than ',MaxAmount,' and more than 1)');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Validating the correct input data type
    Try
      Readln(Amount);
    Except
      Writeln('Wrong input of amount of numbers! It must be an integer');
      flag:= True;
    End;

    //Validate Range
    if ((Amount > MaxAmount) or (Amount <= 1)) and (flag = False) then
    begin
      Writeln('Wrong input of amount of numbers! It must be >1 and <=',MaxAmount);
      flag:= True;
    end;

  Until flag = False;

  //Declaring available symbols and their value
  Writeln;
  Writeln('Available symbols on the ',NS,'th number system and their number system:');
  for i := 0 to (NS - 1) do
    Writeln('Symbol ', NSAlphabet[i+1],' Value = ',i);
  Writeln;

  Writeln('Enter numbers (no more than ',MaxSize,' digits).');

  //Cycle with postcondition for entering correct data.
  Repeat

    //Initialize the flag
    flag:= False;

    //Read the first entered number and check for correctness.
    Readln(str);

    //Find length of the first number
    Len1:= length(str);

    //Checking the correct length
    if Len1 > MaxSize then
    begin
      Writeln('Wrong input of number! The length of the number must be no more than ',MaxSize,' digits.');
      flag:= True;
    end

    //Else if length >1, the first digit cannot be 0 (in the mirrored view it is last)
    else if (Len1 > 1) and (str[1] = '0') then
    begin
      Writeln('Wrong input of number! The first digit of a number cannot be 0');
      flag:= True;
    end

    //Else writing a number to an array and checking for valid symbols
    else
    begin

      //Reset the first number for the input
      for i := 1 to length(Num1) do
        Num1[i]:= 0;

      //Write the first entered number in mirrored view to an array
      i:=1;
      while (i <= Len1) and (flag = False) do
      begin

        //Transfer to numerical value (-1 because numbering in delphi starts from 1)
        Num1[i]:= Pos(str[Len1-i+1], NSAlphabet) - 1;

        //Checking for correct input in the number system
        //Num1[i] will be <0 if the symbol is not in NSAlphabet
        if (Num1[i] < 0) or (Num1[i] >= NS) then
        begin
          Writeln('Wrong input of number! Namely, wrong input of symbols! See available symbols above!');
          flag:= True;
        end;

        //Modernize i
        i:= i + 1;
      end;

    end;

  Until flag = False;



  //The cycle go (Amount-1) times to multiply of all the numbers
  for j := 1 to (Amount - 1) do
  begin

    Writeln('*');

    //Cycle with postcondition for entering correct data.
    Repeat

      //Initialize the flag
      flag:= False;

      //Read the second entered number and check for correctness.
      Readln(str);

      //Find length of the second number
      Len2:= length(str);

      //Checking the correct length
      if Len2 > MaxSize then
      begin
        Writeln('Wrong input of number! The length of the number must be no more than ',MaxSize,' digits.');
        flag:= True;
      end

      //Else if length >1, the first digit cannot be 0 (in the mirrored view it is last)
      else if (Len2 > 1) and (str[1] = '0') then
      begin
        Writeln('Wrong input of number! The first digit of a number cannot be 0');
        flag:= True;
      end

      //Else writing a number to an array and checking for valid symbols
      else
      begin

        //Reset the second number for the input
        for i := 1 to length(Num2) do
          Num2[i]:= 0;

        //Write the second number in mirrored view to an array
        i:=1;
        while (i <= Len2) and (flag = False) do
        begin

          //Transfer to numerical value (-1 because numbering in delphi starts from 1)
          Num2[i]:= Pos(str[Len2-i+1], NSAlphabet) - 1;

          //Checking for correct input in the number system
          //Num2[i] will be <0 if the symbol is not in NSAlphabet
          if (Num2[i] < 0) or (Num2[i] >= NS) then
          begin
            Writeln('Wrong input of number! Namely, wrong input of symbols! See available symbols above!');
            flag:= True;
          end;

          //Modernize i
          i:= i + 1;
        end;

      end;

    Until flag = False;

    //Multiply the digits of the first number by the second number
    for i := 1 to Len1 do
    begin
      for k := 1 to Len2 do
      begin

        //Сalculate at what position in the multiplication the element now
        PosElement:= k + i - 1;

        //Starting to multiply the last digits of the numbers (in the mirrored view it is first)
        //and add the carry (if there is).
        Prod:= Num1[i] * Num2[k] + CarryProd;

        //The integer part of dividing by NS is the carry that will go to the next element
        CarryProd:= Prod div NS;

        //Find the sum of digits in a current position element
        Sum:= CarrySum + ProdNums[PosElement] + (Prod mod NS);

        //The integer part of dividing by NS is the carry that will go to the next element
        CarrySum:= Sum div NS;

        //The modulo of the Sum by NS is the digit in the ProdNums
        ProdNums[PosElement] := Sum mod NS;

        //If there is a carry on the last digit of the second number, then add a carry to the next element
        if k = Len2 then
        begin

          if CarryProd >= 1 then
          begin

            //ProdNums in the next element is equal to the carry, since this element is new for multiplied
            ProdNums[PosElement+1]:=CarryProd;

            //Carry is assigned 0 for the next iterations
            CarryProd:=0;
          end;

          if CarrySum = 1 then
          begin

            //ProdNums in the next element is equal to the sum of carry and ProdNums in the next element,
            //since this element was already for added
            ProdNums[PosElement+1]:= CarrySum + ProdNums[PosElement+1];

            //Carry is assigned 0 for the next iterations
            CarrySum:=0;
          end;

        end;
      end;

      //If in the last digits of the two numbers there is a number in the next position,
      //then the current position element increases
      if (i = Len1) and (ProdNums[PosElement+1] > 0) then
        PosElement:= PosElement + 1;

    end;

    //For the next iteration, the first number becomes the product of the previous.
    //And reset the produs of nums for the next iteration
    for i := 1 to PosElement do
    begin
      Num1[i]:= ProdNums[i];
      ProdNums[i]:= 0;
    end;

    //Update the length of the first number
    Len1:= PosElement;

    //Reset the carryes for the operations
    CarrySum:= 0;
    CarryProd:= 0;

  end;






  Writeln('The prod of the numbers is:');

  //If the product is 0, then output one 0
  if Num1[PosElement] = 0 then
    Writeln(Num1[PosElement])

  //Else write the answer, mirroring the back
  //And transfer to symbolic value (+1 because numbering in delphi starts from 1)
  else
    for i := PosElement downto 1 do
      Write(NSAlphabet[Num1[i]+1]);

  Readln;
End.
```