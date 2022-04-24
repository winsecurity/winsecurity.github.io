# Marshalling in C#


### Select Unicode version of function in DllImport
we can select unicode version from dllimport directly and it takes unicode version of function by default

```csharp
[DllImport("Kernel32.dll",CharSet=CharSet.Unicode)]
```


### Handle HWND

### DWORD
```csharp
System.UInt32
```

### LPDWORD
we need to pass by reference 

```csharp
ref UInt32

GetUsernameW(sb,ref nsize)
```

### LPWSTR
```csharp
[param:MarshalAs(UnmanagedType.LPWStr)] string lpText
```

### NULL
```csharp
IntPtr.Zero
```

### BOOL
Except zero, every other number is treated as TRUE

BOOL defines as integer 
```csharp
[return:MarshalAs(UnmanagedType.Bool)]

System.Int32

System.Boolean
```

Example is below
```csharp
[DllImport("Kernel32.dll")]
[return: MarshalAs(UnmanagedType.Bool)]
public static extern bool CloseHandle(
    IntPtr hObject
);
```

### BOOLEAN
BOOLEAN is byte 

```csharp
System.Boolean
```

