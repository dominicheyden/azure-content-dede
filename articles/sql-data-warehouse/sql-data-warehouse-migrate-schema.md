<properties
   pageTitle="Migrieren Ihres Schemas nach SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration eines Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="03/23/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Migrieren Ihres Schemas nach SQL Data Warehouse#

In den folgenden Übersichten erhalten Sie Informationen zu den Unterschieden zwischen SQL Server und SQL Data Warehouse, sodass Sie Ihre Datenbank migrieren können.

### Funktionen in Tabellen
In SQL Data Warehouse werden die folgenden Funktionen nicht verwendet oder nicht unterstützt:

- Primärschlüssel
- Fremdschlüssel
- CHECK-Einschränkungen
- UNIQUE-Einschränkungen
- Eindeutige Indizes
- Berechnete Spalten
- Spalten mit geringer Dichte
- Benutzerdefinierte Typen
- Indizierte Sichten
- Identitäten
- Sequenzen
- Trigger
- Synonyme

### Datentypunterschiede
In SQL Data Warehouse werden die folgenden allgemeinen Geschäftsdatentypen unterstützt:

- bigint
- binary
- Bit
- char
- date
- datetime
- datetime2
- datetimeoffset
- decimal
- float
- int
- money
- nchar
- nvarchar
- real
- smalldatetime
- smallint
- smallmoney
- in
- tinyint
- varbinary
- varchar

Mit der folgenden Abfrage können Sie Spalten in Ihrem Data Warehouse ermitteln, die inkompatible Datentypen enthalten:

```sql
SELECT  t.[name]
,       c.[name]
,       c.[system_type_id]
,       c.[user_type_id]
,       y.[is_user_defined]
,       y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN
                (   'geography'
                ,   'geometry'
                ,   'hierarchyid'
                ,   'image'
                ,   'ntext'
                ,   'numeric'
                ,   'sql_variant'
                ,   'sysname'
                ,   'text'
                ,   'timestamp'
                ,   'uniqueidentifier'
                ,   'xml'
                )

OR  (   y.[name] IN (  'nvarchar','varchar','varbinary')
    AND c.[max_length] = -1
    )
OR  y.[is_user_defined] = 1
;

```

Die Abfrage enthält alle benutzerdefinierten Datentypen, die auch nicht unterstützt werden.

Wenn Ihre Datenbank nicht unterstützte Datentypen enthält, ist das kein Grund zur Sorge. Unten sind einige Alternativen aufgeführt, die Sie stattdessen wählen können.

Alternativen:

- **geometry**: ein varbinary-Typ
- **geography**: ein varbinary-Typ
- **hierarchyid**: Dieser CLR-Typ wird nicht unterstützt.
- **image**, **text**, **ntext**: varchar/nvarchar (je kleiner, desto besser)
- **nvarchar(max)**: nvarchar(4000) oder kleiner zur Verbesserung der Leistung
- **numeric**: decimal
- **sql\_variant**: Spalte in mehrere Spalten mit starker Typisierung unterteilen
- **sysname**: nvarchar(128)
- **table**: in temporäre Tabellen konvertieren
- **timestamp**: Code anpassen, sodass datetime2 und die `CURRENT_TIMESTAMP`-Funktion verwendet wird. Beachten Sie, dass Sie "current\_timestamp" nicht als Standardeinschränkung verwenden können und dass der Wert nicht automatisch aktualisiert wird. Wenn Sie rowversion-Werte aus einer Spalte mit timestamp-Typ migrieren müssen, sollten Sie binary(8) oder varbinary(8) für NOT NULL- oder NULL-Zeilenversionswerte verwenden.
- **varchar(max)**: varchar(8000) oder kleiner zur Verbesserung der Leistung
- **uniqueidentifier**: varbinary(16) oder varchar(36), abhängig vom Eingabeformat (Binärdaten oder Zeichen) Ihrer Werte. Wenn das Eingabeformat auf Zeichen basiert, ist eine Optimierung möglich. Durch das Konvertieren von Zeichen in das Binärformat können Sie den Spaltenspeicher um über 50 Prozent reduzieren. In sehr großen Tabellen kann diese Optimierung von Vorteil sein.
- **Benutzerdefinierte Datentypen**: zurück in systemeigene Typen konvertieren, falls möglich
- **xml**: varchar(8000) oder kleiner zur Verbesserung der Leistung. Gegebenenfalls in Spalten unterteilen.

Teilweise unterstützt:

- Standardeinschränkungen unterstützen nur Literale und Konstanten. Nicht deterministische Ausdrücke oder Funktionen, z. B. `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt.

> [AZURE.NOTE] Definieren Sie Ihre Tabellen so, dass die maximal mögliche Zeilengröße, einschließlich der vollständigen Länge der Spalten mit variabler Länge, 32.767 Byte nicht überschreitet. Sie können zwar eine Zeile mit Daten variabler Länge definieren, bei der dieser Wert überschritten wird, aber Sie können keine Daten in die Tabelle einfügen. Versuchen Sie außerdem, die Größe Ihrer Spalten mit variabler Länge zu beschränken, um beim Ausführen von Abfragen einen noch besseren Durchsatz zu erzielen.

## Nächste Schritte
Nachdem Sie Ihr Datenbankschema erfolgreich in SQLDW migriert haben, können Sie mit den folgenden Artikeln fortfahren:

- [Migrieren Ihrer Daten][]
- [Migrieren Ihres Codes][]

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][].

<!--Image references-->

<!--Article references-->
[Migrieren Ihres Codes]: sql-data-warehouse-migrate-code.md
[Migrieren Ihrer Daten]: sql-data-warehouse-migrate-data.md
[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0330_2016-->