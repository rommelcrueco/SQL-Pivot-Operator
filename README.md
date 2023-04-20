##### Tutorial from:
https://www.sqlservertutorial.net/sql-server-basics/sql-server-pivot/

##### Pre-requisites:
1. SQL server installed on your machine
2. SSMS

##### Steps:
1. Create the database and tables including the data by executing the following scripts:
```sql

USE [master]
GO
/****** Object:  Database [SqlPivotOperation]    Script Date: 4/19/2023 8:16:49 PM ******/
CREATE DATABASE [SqlPivotOperation]
 CONTAINMENT = NONE
 ON  PRIMARY 
( NAME = N'SqlPivotOperation', FILENAME = N'C:\SQLDB\Data\SqlPivotOperation.mdf' , SIZE = 8192KB , MAXSIZE = UNLIMITED, FILEGROWTH = 65536KB )
 LOG ON 
( NAME = N'SqlPivotOperation_log', FILENAME = N'C:\SQLDB\Log\SqlPivotOperation_log.ldf' , SIZE = 8192KB , MAXSIZE = 2048GB , FILEGROWTH = 65536KB )
 WITH CATALOG_COLLATION = DATABASE_DEFAULT
GO
ALTER DATABASE [SqlPivotOperation] SET COMPATIBILITY_LEVEL = 150
GO
IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [SqlPivotOperation].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO
ALTER DATABASE [SqlPivotOperation] SET ANSI_NULL_DEFAULT OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET ANSI_NULLS OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET ANSI_PADDING OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET ANSI_WARNINGS OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET ARITHABORT OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET AUTO_CLOSE OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET AUTO_SHRINK OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET AUTO_UPDATE_STATISTICS ON 
GO
ALTER DATABASE [SqlPivotOperation] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET CURSOR_DEFAULT  GLOBAL 
GO
ALTER DATABASE [SqlPivotOperation] SET CONCAT_NULL_YIELDS_NULL OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET NUMERIC_ROUNDABORT OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET QUOTED_IDENTIFIER OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET RECURSIVE_TRIGGERS OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET  DISABLE_BROKER 
GO
ALTER DATABASE [SqlPivotOperation] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET TRUSTWORTHY OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET ALLOW_SNAPSHOT_ISOLATION OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET PARAMETERIZATION SIMPLE 
GO
ALTER DATABASE [SqlPivotOperation] SET READ_COMMITTED_SNAPSHOT OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET HONOR_BROKER_PRIORITY OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET RECOVERY FULL 
GO
ALTER DATABASE [SqlPivotOperation] SET  MULTI_USER 
GO
ALTER DATABASE [SqlPivotOperation] SET PAGE_VERIFY CHECKSUM  
GO
ALTER DATABASE [SqlPivotOperation] SET DB_CHAINING OFF 
GO
ALTER DATABASE [SqlPivotOperation] SET FILESTREAM( NON_TRANSACTED_ACCESS = OFF ) 
GO
ALTER DATABASE [SqlPivotOperation] SET TARGET_RECOVERY_TIME = 60 SECONDS 
GO
ALTER DATABASE [SqlPivotOperation] SET DELAYED_DURABILITY = DISABLED 
GO
ALTER DATABASE [SqlPivotOperation] SET ACCELERATED_DATABASE_RECOVERY = OFF  
GO
EXEC sys.sp_db_vardecimal_storage_format N'SqlPivotOperation', N'ON'
GO
ALTER DATABASE [SqlPivotOperation] SET QUERY_STORE = OFF
GO
USE [SqlPivotOperation]
GO
/****** Object:  Schema [production]    Script Date: 4/19/2023 8:16:50 PM ******/
CREATE SCHEMA [production]
GO
/****** Object:  Table [production].[categories]    Script Date: 4/19/2023 8:16:50 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [production].[categories](
	[category_id] [int] IDENTITY(1,1) NOT NULL,
	[category_name] [nvarchar](50) NOT NULL,
 CONSTRAINT [PK_categories] PRIMARY KEY CLUSTERED 
(
	[category_id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
/****** Object:  Table [production].[products]    Script Date: 4/19/2023 8:16:50 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [production].[products](
	[product_id] [int] IDENTITY(1,1) NOT NULL,
	[product_name] [nvarchar](50) NOT NULL,
	[brand_id] [int] NOT NULL,
	[category_id] [int] NOT NULL,
	[model_year] [int] NOT NULL,
	[list_price] [decimal](18, 2) NOT NULL,
 CONSTRAINT [PK_production.products] PRIMARY KEY CLUSTERED 
(
	[product_id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
SET IDENTITY_INSERT [production].[categories] ON 

INSERT [production].[categories] ([category_id], [category_name]) VALUES (1, N'Children Bicycles')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (2, N'Comfort Bicycles')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (3, N'Cruisers Bicycles')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (4, N'Cyclocross Bicycles')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (5, N'Electric Bikes')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (6, N'Mountain Bikes')
INSERT [production].[categories] ([category_id], [category_name]) VALUES (7, N'Road Bikes')
SET IDENTITY_INSERT [production].[categories] OFF
GO
SET IDENTITY_INSERT [production].[products] ON 

INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (1, N'test', 1, 1, 2016, CAST(100.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (2, N'test', 1, 1, 2016, CAST(125.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (3, N'test', 1, 2, 2017, CAST(130.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (4, N'test', 1, 2, 2016, CAST(125.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (5, N'test', 1, 2, 2018, CAST(140.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (6, N'test', 1, 3, 2016, CAST(150.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (7, N'test', 1, 3, 2015, CAST(140.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (8, N'test', 1, 4, 2018, CAST(155.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (9, N'test', 1, 5, 2019, CAST(160.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (10, N'test', 1, 5, 2019, CAST(165.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (11, N'test', 1, 5, 2020, CAST(165.00 AS Decimal(18, 2)))
INSERT [production].[products] ([product_id], [product_name], [brand_id], [category_id], [model_year], [list_price]) VALUES (12, N'test', 1, 5, 2021, CAST(170.00 AS Decimal(18, 2)))
SET IDENTITY_INSERT [production].[products] OFF
GO
ALTER TABLE [production].[products]  WITH CHECK ADD  CONSTRAINT [FK_products_categories] FOREIGN KEY([category_id])
REFERENCES [production].[categories] ([category_id])
GO
ALTER TABLE [production].[products] CHECK CONSTRAINT [FK_products_categories]
GO
USE [master]
GO
ALTER DATABASE [SqlPivotOperation] SET  READ_WRITE 
GO


```

2. To simulate, follow the steps in https://www.sqlservertutorial.net/sql-server-basics/sql-server-pivot/