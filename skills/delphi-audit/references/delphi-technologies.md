# Delphi Technology Reference

## Delphi Version History

| Version | Year | Commercial Name | Status |
|--------|------|----------------|--------|
| Delphi 1 | 1995 | Delphi 1 | ❌ Obsolete |
| Delphi 2 | 1996 | Delphi 2 | ❌ Obsolete |
| Delphi 3 | 1997 | Delphi 3 | ❌ Obsolete |
| Delphi 4 | 1998 | Delphi 4 | ❌ Obsolete |
| Delphi 5 | 1999 | Delphi 5 | ❌ Obsolete |
| Delphi 6 | 2001 | Delphi 6 | ❌ Obsolete |
| Delphi 7 | 2002 | Delphi 7 | ⚠️ Widely used (legacy) |
| Delphi 8 | 2003 | Delphi 8 (.NET) | ❌ Discontinued |
| Delphi 2005 | 2004 | Delphi 9 | ❌ Obsolete |
| Delphi 2006 | 2005 | Delphi 10 | ❌ Obsolete |
| Delphi 2007 | 2007 | Delphi 11 | ⚠️ Legacy |
| Delphi 2009 | 2008 | Delphi 12 | ⚠️ Native Unicode |
| Delphi 2010 | 2009 | Delphi 14 | ⚠️ Legacy |
| Delphi XE | 2010 | Delphi 15 | ⚠️ Legacy |
| Delphi XE2 | 2011 | Delphi 16 | ⚠️ Legacy (FMX introduced) |
| Delphi XE3–XE8 | 2012–2015 | — | ⚠️ Legacy |
| Delphi 10 Seattle | 2015 | Delphi 20 | ⚠️ Support ended |
| Delphi 10.1 Berlin | 2016 | — | ⚠️ Support ended |
| Delphi 10.2 Tokyo | 2017 | — | ⚠️ Support ended |
| Delphi 10.3 Rio | 2018 | — | ⚠️ Support ended |
| Delphi 10.4 Sydney | 2020 | — | ✅ Supported |
| Delphi 11 Alexandria | 2021 | — | ✅ Supported |
| Delphi 12 Athens | 2023 | — | ✅ Current/Recommended |

---

## Data Access Components

### BDE (Borland Database Engine)
- **Status**: ❌ OBSOLETE — DO NOT USE
- **Problem**: Not officially supported on Windows 8+ without compatibility hacks
- **Supported databases**: Paradox, dBase, Interbase (via ODBC)
- **Risk**: Silent failures on Windows 10/11, doesn't work in 64-bit
- **Recommended migration**: FireDAC

### ADO (ActiveX Data Objects)
- **Status**: ⚠️ LEGACY — Works but not evolving
- **Components**: TADOConnection, TADOQuery, TADOTable, TADOStoredProc
- **Supported databases**: SQL Server, Access, Oracle, MySQL (via ODBC)
- **Risk**: Dependency on ODBC drivers, no support for modern features
- **Recommended migration**: FireDAC

### DBExpress
- **Status**: ⚠️ LEGACY — Present until Delphi 12, but not evolved
- **Components**: TSQLConnection, TSQLQuery, TSQLDataSet
- **Supported databases**: Interbase, MySQL, Oracle, DB2, MSSQL
- **Recommended migration**: FireDAC

### IBX (InterBase Express)
- **Status**: ⚠️ LEGACY for older versions; ✅ active in IBX for Firebird (Firebird Pascal)
- **Specific for**: InterBase and Firebird
- **Current alternative**: MWA Software's IBX (open source) or FireDAC

### FireDAC
- **Status**: ✅ CURRENT — Recommended by Embarcadero
- **Available since**: Delphi XE3 (as AnyDAC), renamed in XE5
- **Components**: TFDConnection, TFDQuery, TFDTable, TFDStoredProc, TFDTransaction
- **Supported databases**: Firebird, Oracle, MSSQL, MySQL/MariaDB, SQLite, PostgreSQL, MongoDB, Interbase, DB2, Informix, and more
- **Advantages**: High performance, 64-bit support, array DML, local SQL, offline caching

### Zeos (ZeosLib)
- **Status**: ✅ Open Source — Community maintained
- **Components**: TZConnection, TZQuery, TZTable
- **Supported databases**: PostgreSQL, MySQL, Firebird, SQLite, Oracle, MSSQL
- **Common use**: Projects that don't have a FireDAC license

### UniDAC (Devart)
- **Status**: ✅ Commercial — Well maintained
- **Supported databases**: Wide range, including cloud databases

---

## UI Components (VCL)

### Native Embarcadero/Borland
- TDBGrid, TDBEdit, TDBNavigator — standard, no extra license
- TPageControl, TTabSheet, TPanel — basic layout

### DevExpress VCL
- **Status**: ✅ Active and popular
- **Key components**: TcxGrid, TdxRibbon, TdxNavBar, TdxSpreadSheet
- **Note**: Verify installed version vs. Delphi version compatibility

### TMS Software
- **Status**: ✅ Active
- **Key components**: TTMSFNCGrid, TAdvStringGrid, TDBAdvGrid

### Raize Components / Konopka
- **Status**: ⚠️ Support ended — included in Delphi XE8+

### JVCL (JEDI VCL)
- **Status**: ✅ Open Source — active community
- **Key components**: TJvDBGrid, TJvValidateEdit, TJvXPBar

---

## Reporting Frameworks

### FastReport
- **Status**: ✅ Active and popular
- **Versions**: FastReport 4 (legacy), FastReport 5, FastReport 6
- **Note**: Older versions are not compatible with Delphi 12

### ReportBuilder (Digital Metaphors)
- **Status**: ⚠️ Active but less popular
- **Usage**: More common in North American projects

### Rave Reports
- **Status**: ❌ Discontinued (was native in Delphi 7/2007)

### QuickReport
- **Status**: ❌ Practically obsolete

### ACBr Report (NFe/NFCe)
- **Status**: ✅ Active — Brazilian open source project

---

## Popular Frameworks and Libraries

### ACBr
- **Status**: ✅ WIDELY USED in Brazil
- **Purpose**: NFe, NFCe, CTe, MDFe, SAT, TEF, Boleto, SPED, eSocial
- **License**: Open Source (LGPL)

### Boss (Package Manager)
- **Status**: ✅ Modern — package manager for Delphi
- **Usage**: Manage dependencies via command line

### Horse (Web Framework)
- **Status**: ✅ Active — HTTP framework for REST APIs in Delphi
- **Usage**: Backend/API development in Delphi

### mORMot
- **Status**: ✅ Active — ORM, REST, SOA framework
- **Usage**: Systems requiring high performance and ORM

### Spring4D
- **Status**: ✅ Active — dependency injection framework
- **Usage**: Projects following SOLID patterns

---

## OS and Architecture Compatibility

| Technology | Win XP | Win 7 | Win 10 | Win 11 | 32-bit | 64-bit |
|-----------|--------|-------|--------|--------|--------|--------|
| BDE | ✅ | ⚠️ | ⚠️ hack | ❌ | ✅ | ❌ |
| ADO | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| FireDAC | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Delphi 7 compiled | ✅ | ✅ | ⚠️ | ⚠️ | ✅ | ❌ |
| Delphi 10+ compiled | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Delphi 12 compiled | ❌ | ⚠️ | ✅ | ✅ | ✅ | ✅ |

---

## Technology Risk Guide

### BDE in production → 🚨 CRITICAL RISK
```
Known issues:
- 2GB limit for Paradox/dBase database files
- Incompatibility with Windows 11 without patches
- Impossible to compile for 64-bit
- No official support since Delphi 2007
- High DPI (HiDPI) issues
Action: Migrate to FireDAC URGENTLY
```

### Delphi 7 or earlier → ⚠️ ATTENTION
```
Known issues:
- No native Unicode support (Delphi < 2009)
- Strings are AnsiString — problems with special characters
- No 64-bit support
- Old VCL components with DPI issues
Action: Plan compiler migration
```

### Absence of exception handling → ⚠️ ATTENTION
```
Symptoms: No try/except, direct alerts to DBMS user
Impact: Unstable system, inconsistent data
Action: Progressive refactoring with structured handling
```
