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

### shi2_type
This is member of "SHARE_INFO_2" structure. This field indicates the type of the share.
```csharp
OrderedDictionary SHARE_TYPES = new OrderedDictionary();
SHARE_TYPES.Add(@"STYPE_SPECIAL- Special share reserved for interprocess communication (IPC$) or remote administration of the server", 2147483648); SHARE_TYPES.Add("STYPE_CLUSTER_DFS", 134217728);
SHARE_TYPES.Add("STYPE_TEMPORARY", 1073741824);
SHARE_TYPES.Add("STYPE_CLUSTER_SOFS", 67108864);
SHARE_TYPES.Add("STYPE_CLUSTER_FS", 33554432);
SHARE_TYPES.Add("STYPE_IPC", 3);
SHARE_TYPES.Add("STYPE_DEVICE", 2);
SHARE_TYPES.Add("STYPE_PRINTQ", 1);
SHARE_TYPES.Add("STYPE_DISKTREE", 0);

if ( Int64.Parse(s.shi2_type.ToString())>= Int64.Parse(d.Value.ToString())){
Console.WriteLine(d.Key.ToString());
    break;
}
```
