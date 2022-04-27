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

### LPSTR
```csharp
[param:MarshalAs(UnmanagedType.LPStr)]
```

### LPWSTR, LPCWSTR
```csharp
[param:MarshalAs(UnmanagedType.LPWStr)] string lpText
```

### NULL VOID LPVOID 
we can pass any uninitialized data type	
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


### HKEY
```csharp

HKEY_CLASSES_ROOT = 0x80000000
HKEY_CURRENT_USER =0x80000001
HKEY_LOCAL_MACHINE = 0x80000002
HKEY_USERS = 0x80000003
HKEY_PERFORMANCE_DATA = 0x80000004
HKEY_CURRENT_CONFIG = 0x80000005
HKEY_DYN_DATA = 0x80000006
	
UIntPtr hkcr = new UIntPtr(0x80000000);
UIntPtr hkcu = new UIntPtr(0x80000001);
UIntPtr hklm = new UIntPtr(0x80000002);
UIntPtr hku = new UIntPtr(0x80000003);
UIntPtr hkpd = new UIntPtr(0x80000004);
UIntPtr hkcc = new UIntPtr(0x80000005);
UIntPtr hkdynd = new UIntPtr(0x80000006);	
```

### REGSAM
Convert these into integers and pass into function
```csharp
KEY_ALL_ACCESS (0xF003F)
KEY_CREATE_LINK (0x0020)
KEY_CREATE_SUB_KEY (0x0004)
KEY_ENUMERATE_SUB_KEYS (0x0008)
KEY_EXECUTE (0x20019)
KEY_NOTIFY (0x0010)
KEY_QUERY_VALUE (0x0001)
KEY_READ (0x20019)
KEY_SET_VALUE (0x0002)
KEY_WOW64_32KEY (0x0200)
KEY_WOW64_64KEY (0x0100)
KEY_WRITE (0x20006)
```

### Registry Value Types
```csharp
REG_NONE - 0
REG_SZ - 1
REG_EXPAND_SZ - 2
REG_BINARY - 3	
REG_DWORD - 4
REG_DWORD_LITTLE_ENDIAN - 4
REG_DWORD_BIG_ENDIAN - 5
REG_LINK - 6
REG_MULTI_SZ - 7
REG_RESOURCE_LIST - 8
REG_QWORD - 11 
REG_QWORD_LITTLE_ENDIAN - 11 
```