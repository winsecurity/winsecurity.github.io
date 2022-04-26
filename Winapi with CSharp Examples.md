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

## NetShareEnum
```csharp

internal struct SHARE_INFO_2 {
  [MarshalAs(UnmanagedType.LPWStr)]
  public string shi2_netname;
  public UInt32 shi2_type;
  [MarshalAs(UnmanagedType.LPWStr)]
  public string shi2_remark;
  public UInt32 shi2_permissions;
  public UInt32 shi2_max_uses;
  public UInt32 shi2_current_uses;
  [MarshalAs(UnmanagedType.LPWStr)]
  public string shi2_path;
  [MarshalAs(UnmanagedType.LPWStr)]
  public string shi2_passwd;
}


[DllImport("Netapi32.dll")]
public static extern NET_API_STATUS NetShareEnum(
  [param: MarshalAs(UnmanagedType.LPWStr)] string servername,
  UInt32 level,
  ref IntPtr bufptr,
  UInt32 prefmaxlen,
  ref UInt32 entriesread,
  ref UInt32 totalentries,
  IntPtr resume_handle
);


OrderedDictionary SHARE_TYPES = new OrderedDictionary();
SHARE_TYPES.Add(@ "STYPE_SPECIAL- Special share reserved for interprocess communication (IPC$) or remote administration of the server", 2147483648);
SHARE_TYPES.Add("STYPE_CLUSTER_DFS", 134217728);
SHARE_TYPES.Add("STYPE_TEMPORARY", 1073741824);
SHARE_TYPES.Add("STYPE_CLUSTER_SOFS", 67108864);
SHARE_TYPES.Add("STYPE_CLUSTER_FS", 33554432);
SHARE_TYPES.Add("STYPE_IPC", 3);
SHARE_TYPES.Add("STYPE_DEVICE", 2);
SHARE_TYPES.Add("STYPE_PRINTQ", 1);
SHARE_TYPES.Add("STYPE_DISKTREE", 0);

SHARE_INFO_0 share = new SHARE_INFO_0();
IntPtr bufptr = IntPtr.Zero;
UInt32 entriesread = 0, totalentries = 0;
NET_API_STATUS nas = NetShareEnum(
  computername,
  2,
  ref bufptr,
  2000,
  ref entriesread,
  ref totalentries,
  IntPtr.Zero
);
for (int i = 0; i < totalentries; i++) {
  SHARE_INFO_2 s = (SHARE_INFO_2) Marshal.PtrToStructure(bufptr, typeof (SHARE_INFO_2));
  Console.WriteLine("Share Name: {0}", s.shi2_netname.ToString());
  foreach(DictionaryEntry d in SHARE_TYPES) {
    if (Int64.Parse(s.shi2_type.ToString()) >= Int64.Parse(d.Value.ToString())) {
      Console.WriteLine(d.Key.ToString());
      break;
    }
  }
 Console.WriteLine(s.shi2_type.ToString()); Console.WriteLine(s.shi2_remark.ToString()); Console.WriteLine(s.shi2_permissions.ToString()); Console.WriteLine(s.shi2_current_uses.ToString());
 Console.WriteLine(s.shi2_path.ToString());
  Console.WriteLine(s.shi2_passwd);
  bufptr += Marshal.SizeOf(typeof (SHARE_INFO_2));

}
```

## GetSystemInfo
```csharp
[StructLayout(LayoutKind.Sequential)]
public struct DUMMYSTRUCTNAME {
  public UInt16 wProcessorArchitecture;
  public UInt16 wReserved;
}

[StructLayout(LayoutKind.Sequential)]
public struct SYSTEM_INFO {

  public DUMMYUNIONNAME du;

  [StructLayout(LayoutKind.Explicit)]
  public struct DUMMYUNIONNAME {
	// the member     DWORD dwOemId;
	// became deprecated 
	
    [FieldOffset(0)]
    public DUMMYSTRUCTNAME ds;

  }

  public UInt32 dwPageSize;
  public IntPtr lpMinimumApplicationAddress;
  public IntPtr lpMaximumApplicationAddress;
  public UInt32 dwActiveProcessorMask;
  public UInt32 dwNumberOfProcessors;
  public UInt32 dwProcessorType;
  public UInt32 dwAllocationGranularity;
  public UInt16 wProcessorLevel;
  public UInt16 wProcessorRevision;
}


[DllImport("Kernel32.dll")]
public static extern void GetSystemInfo(
  ref SYSTEM_INFO lpSystemInfo
);

SYSTEM_INFO s2 = new SYSTEM_INFO();

GetSystemInfo(ref s2);
Console.WriteLine(s2.dwNumberOfProcessors);
Console.WriteLine(s2.du.ds.wProcessorArchitecture);
Console.WriteLine(s2.dwProcessorType);
```

