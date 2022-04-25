# Winapi with C# Examples


## MessageBox
```csharp
[DllImport("User32.dll",CharSet=CharSet.Unicode)]

public static extern int MessageBoxW(
	IntPtr HWND,
	[param:MarshalAs(UnmanagedType.LPWStr) string lpText,
	[param:MarshalAs(UnmanagedType.LPWStr) string lpCaption,
	 UInt32 uType
)

MessageBoxW(
IntPtr.Zero, sb1, "Hello", 0
);
```


## GetUserNameW
second parameter is [in,out]. This means this parameter is used for input and also function outputs something into this parameter. To save the results we need to pass as ref.

```csharp
[DllImport("Advapi32.dll",CharSet=CharSet.Unicode)
 
public static extern bool GetUsernameW(
	[param:MarshalAs(UnmanagedType.LPWStr)] StringBuilder lpBuffer,
	ref	UInt32 	pcbBuffer
)

StringBuilder sb = new StringBuilder(100);
bool result = GetUserNameW(sb,100);
Console.WriteLine(sb.ToString());
```

