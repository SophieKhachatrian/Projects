Select *
From HousingData 

-- Make Standard Date Format

Update HousingData 
SET SaleDate = CONVERT(Date,SaleDate)
------------------------------------------------------------------------------------------------------------------------------------------------------------------

-- Populate Property Address data

Select a.ParcelID, b.ParcelID, a.PropertyAddress, b.PropertyAddress
From HousingData a
Join HousingData b 
  On a.ParcelID = b.ParcelID
  and a.UniqueID <> b.UniqueID
Where a.PropertyAddress is NULL

--I replece all these NULL's with their corresponding values

Update a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
From HousingData a
Join HousingData b 
  On a.ParcelID = b.ParcelID
  and a.UniqueID <> b.UniqueID
------------------------------------------------------------------------------------------------------------------------------------------------------------------

-- Breaking out Address into Individual Columns (Address, City, State) In PropertyAddress and OwnerAddress

Select PropertyAddress, Substring(PropertyAddress, 1, CharIndex(',', PropertyAddress)-1) as Addresss,
Substring(PropertyAddress, CharIndex(',', PropertyAddress)+1, Len(PropertyAddress)) as Addresss
From HousingData 

ALTER TABLE HousingData
ADD PropAdressOnly nvarchar(255)

ALTER TABLE HousingData
ADD PropCity nvarchar(255)

Update HousingData
SET PropAdressOnly = Substring(PropertyAddress, 1, CharIndex(',', PropertyAddress)-1)
Update HousingData
SET PropCity = Substring(PropertyAddress, CharIndex(',', PropertyAddress)+1, Len(PropertyAddress))


Select
PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3),
PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2),
PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)
From  HousingData


ALTER TABLE HousingData
Add OwnerSplitAddress Nvarchar(255);


Update  HousingData
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)


ALTER TABLE  HousingData
Add OwnerCity Nvarchar(255);

Update  HousingData
SET OwnerCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)


ALTER TABLE  HousingData
Add OwnerSplitState Nvarchar(255);

Update  HousingData
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

-- Change 1 and 0 to Yes and No in "Sold as Vacant" field
--This is how it is
Select  Distinct(SoldAsVacant), Count(SoldAsVacant)
FROM HousingData
Group by SoldAsVacant

ALTER TABLE HousingData
ADD SoldVacant nvarchar(3)

UPDATE HousingDATA
SET SoldVacant = CASE WHEN SoldAsVacant = 1 THEN 'YES' ELSE 'NO' END

ALTER TABLE HousingData
DROP COLUMN SoldAsVacant
--This is the end result
Select  Distinct(SoldVacant), Count(SoldVacant)
FROM HousingData
Group by SoldVacant
--------------------------------------------------------------------------------------------------------------------------------------------------------------------

-- Remove Duplicates
WITH RowsCTE AS(
Select *,
ROW_NUMBER() OVER(Partition by ParcelID, PropertyAddress, SalePrice, 
SaleDate, LegalReference Order BY UniqueID) row_num
FROM HousingData
)
DELETE
From RowsCTE
Where row_num > 1
------------------------------------------------------------------------------------------------------------------------------------------------------------------------

--Delete Columns which I won't use

ALTER TABLE HousingData
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate

