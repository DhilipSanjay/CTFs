# Day 18 - The Bits of Christmas

**Date:** 18, December, 2020

**Author:** Dhilip Sanjay S

---
## [.NET Framework](https://dotnet.microsoft.com/)
- Due to its compatibility and long history, the .NET Framework is a popular platform for software developers to develop software with. Anything Windows or web, .NET will cover it.
- .NET Framework can be disassembled using tools like:
    - [ILSpy](https://github.com/icsharpcode/ILSpy)
    - [Dotpeek](https://www.jetbrains.com/decompiler/)

- Remmina RDP
    - To install: `apt install remmina`
    - Enter the credentials and MACHINE_IP. Change to color depth to `RemoteFX (32bpp)`


## Solutions
Open the "TBFC_APP" application in **ILspy or dotpeek** and begin decompiling the code

### What is Santa's password?
- **Answer:** santapassword321
- **Steps to Reproduce:** 
    - Go to CrackMe -> MainForm
    - Expand the `buttonActivate_Click` function.

    ```c#
    private unsafe void buttonActivate_Click(object sender, EventArgs e)
	{
		IntPtr value = Marshal.StringToHGlobalAnsi(textBoxKey.Text);
		sbyte* ptr = (sbyte*)System.Runtime.CompilerServices.Unsafe.AsPointer(ref <Module>.??_C@_0BB@IKKDFEPG@santapassword321@);
		void* ptr2 = (void*)value;
		byte b = *(byte*)ptr2;
		byte b2 = 115;
		if ((uint)b >= 115u)
		{
			while ((uint)b <= (uint)b2)
			{
				if (b != 0)
				{
					ptr2 = (byte*)ptr2 + 1;
					ptr++;
					b = *(byte*)ptr2;
					b2 = (byte)(*ptr);
					if ((uint)b < (uint)b2)
					{
						break;
					}
					continue;
				}
				MessageBox.Show("Welcome, Santa, here's your flag thm{046af}", "That's the right key!", MessageBoxButtons.OK, MessageBoxIcon.Asterisk);
				return;
			}
		}
		MessageBox.Show("Uh Oh! That's the wrong key", "You're not Santa!", MessageBoxButtons.OK, MessageBoxIcon.Hand);
	}
    
    ```
---

### Now that you've retrieved this password, try to login...What is the flag?
- **Answer:** thm{046af}
- **Steps to Reproduce:** 
    - You can see the flag in the above code.
    - You can also type in the password to login into the application.
---

[Back to Advent of Cyber 2](/TryHackMe/Advent%20of%20Cyber%202) 