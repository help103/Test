using System;
using System.Data;
using System.Collections;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

using CMS.PortalControls;
using CMS.Helpers;
using CMS.DataEngine;
using CMS.FormEngine;
using CMS.CustomTables;

/// <summary>
/// Extends the CMSTransformation partial class.
/// </summary>

public partial class CMSWebParts_FeaturedCar : CMSAbstractWebPart
{
    #region "Properties"



    #endregion


    #region "Methods"

    /// <summary>
    /// Content loaded event handler.
    /// </summary>
    protected void Page_Load()
    {
        string manuName = GetAutoManufaturerNameById(DataHelper.GetNotZero(GetValue("ManuName"), 0));
        Label1.Text = manuName;
        Label2.Text = DataHelper.GetNotEmpty(GetValue("Model"), null);
        Label3.Text = DataHelper.GetNotEmpty(GetValue("Description"), null);       
        Label4.Text = GetString("AutoManuName");
        CarImage.ImageUrl = DataHelper.GetNotEmpty(GetValue("Image"), null);       
    }

    private string GetAutoManufaturerNameById(int manuId)
    {
        if (manuId != 0)
        {
            // Prepares the code name (class name) of the custom table
            string customTableClassName = "customtable.AutoManuNames";

            // Gets the custom table
            DataClassInfo customTable = DataClassInfoProvider.GetDataClassInfo(customTableClassName);
            if (customTable == null)
            {
                return string.Empty;
            }
            // Gets a custom table record with a specific primary key ID (25)
            CustomTableItem item1 = CustomTableItemProvider.GetItem(manuId, customTableClassName);
           
            // Loads a string value from the 'ItemText' field of the 'item1' custom table record
            if (CMS.DocumentEngine.DocumentContext.CurrentDocument.DocumentCulture != "ar-AE")
            {
                return ValidationHelper.GetString(item1.GetValue("ManuName_EN"), "");
            }
            else 
            {
                return ValidationHelper.GetString(item1.GetValue("ManuName_AR"), "");
            }
        }

        return string.Empty;
    }

    #endregion
}



