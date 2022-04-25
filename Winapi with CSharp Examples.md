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

## shi2_type
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