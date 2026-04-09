# Component Prefixes — VCL and FMX

## General Rule

Every component **referenced via source code** must be renamed with:
`[prefix][DescriptiveNameInCamelCase]`

Components not referenced via code: default names are tolerable.

```pascal
// ❌ WRONG — default names
Button1.Enabled := False;
Edit1.Text := '';
Label1.Caption := 'Loading...';

// ✅ CORRECT — renamed with prefix
btnSave.Enabled := False;
edtCustomerName.Text := '';
lblStatus.Caption := 'Loading...';
```

---

## Prefix Table

### Data Entry
| Component | Prefix | Examples |
|---|---|---|
| TEdit / TFMXEdit | `edt` | `edtName`, `edtCPF`, `edtSaleValue` |
| TMemo / TFMXMemo | `mmo` | `mmoNotes`, `mmoLog` |
| TComboBox | `cbx` | `cbxState`, `cbxPaymentType` |
| TListBox | `lst` | `lstItems`, `lstCustomers` |
| TCheckBox | `chk` | `chkActive`, `chkAgreeTerms` |
| TRadioButton | `rdb` | `rdbMale`, `rdbFemale` |
| TDateTimePicker | `dtp` | `dtpBirthDate`, `dtpDueDate` |
| TSpinEdit | `spe` | `speQuantity`, `speInstallmentCount` |
| TTrackBar | `trk` | `trkVolume`, `trkBrightness` |
| TNumberBox (FMX) | `nbx` | `nbxValue`, `nbxQuantity` |

### Display
| Component | Prefix | Examples |
|---|---|---|
| TLabel | `lbl` | `lblTitle`, `lblStatus`, `lblTotal` |
| TImage | `img` | `imgLogo`, `imgPhoto`, `imgIcon` |
| TProgressBar | `prg` | `prgLoading`, `prgUpload` |

### Buttons
| Component | Prefix | Examples |
|---|---|---|
| TButton | `btn` | `btnSave`, `btnCancel`, `btnNew` |
| TBitBtn | `bbt` | `bbtConfirm`, `bbtDelete` |
| TSpeedButton | `spb` | `spbPrint`, `spbExport` |
| TToolButton | `tbn` | `tbnSave`, `tbnOpen` |

### Layout and Containers
| Component | Prefix | Examples |
|---|---|---|
| TPanel | `pnl` | `pnlHeader`, `pnlFooter`, `pnlFilters` |
| TGroupBox | `grp` | `grpPersonalData`, `grpAddress` |
| TPageControl | `pgc` | `pgcMain`, `pgcRegistration` |
| TTabSheet | `tbs` | `tbsGeneralData`, `tbsAddress` |
| TScrollBox | `scb` | `scbContent`, `scbItems` |
| TSplitter | `spl` | `splVertical`, `splHorizontal` |
| TFrame | `frm` | `frmFilters`, `frmFooter` |
| TLayout (FMX) | `lyt` | `lytHeader`, `lytButtons` |
| TRectangle (FMX) | `rct` | `rctBackground`, `rctBlueBorder` |

### Grids and Lists
| Component | Prefix | Examples |
|---|---|---|
| TDBGrid | `grd` | `grdCustomers`, `grdOrders`, `grdItems` |
| TStringGrid | `sgr` | `sgrData`, `sgrSummary` |
| TListView | `lsv` | `lsvFiles`, `lsvResults` |
| TTreeView | `trv` | `trvMenu`, `trvCategories` |

### Data
| Component | Prefix | Examples |
|---|---|---|
| TDataSource | `dts` | `dtsCustomers`, `dtsOrders` |
| TFDQuery / TQuery | `qry` | `qryCustomers`, `qryOrders`, `qryAux` |
| TFDTable / TTable | `tbl` | `tblProducts`, `tblInventory` |
| TFDConnection | `cnn` | `cnnMain`, `cnnReport` |
| TFDTransaction | `trn` | `trnMain`, `trnBatch` |
| TClientDataSet | `cds` | `cdsCustomers`, `cdsOrders` |
| TFDMemTable | `mdt` | `mdtResults`, `mdtTemp` |

### Menu and Toolbar
| Component | Prefix | Examples |
|---|---|---|
| TMainMenu | `mnu` | `mnuMain` |
| TPopupMenu | `pmu` | `pmuGrid`, `pmuIcon` |
| TMenuItem | `mni` | `mnuFile`, `mniSave` |
| TToolBar | `tlb` | `tlbMain`, `tlbFormat` |

### Dialogs
| Component | Prefix | Examples |
|---|---|---|
| TOpenDialog | `odlg` | `odlgFile`, `odlgImage` |
| TSaveDialog | `sdlg` | `sdlgReport`, `sdlgExport` |
| TColorDialog | `cdlg` | `cdlgColor` |
| TFontDialog | `fdlg` | `fdlgText` |

### Timers and Communication
| Component | Prefix | Examples |
|---|---|---|
| TTimer | `tmr` | `tmrRefresh`, `tmrSession` |
| TRESTClient | `rtc` | `rtcAPI`, `rtcIfood` |
| TRESTRequest | `rtr` | `rtrGetOrders`, `rtrAuthenticate` |
| TRESTResponse | `rsp` | `rspOrders`, `rspToken` |

### FMX Specific
| Component | Prefix | Examples |
|---|---|---|
| TForm (FMX) | `frm` | `frmMain`, `frmRegistration` |
| TTabControl (FMX) | `tbc` | `tbcNavigation` |
| TTabItem (FMX) | `tbi` | `tbiHome`, `tbiSettings` |
| TSwitch (FMX) | `swt` | `swtActive`, `swtNotification` |
| TCalendar (FMX) | `cal` | `calDueDate` |
