import arcpy
def perform_threshold():
    layer = arcpy.GetParameterAsText(0)
    field_name = arcpy.GetParameterAsText(1)
    threshold = arcpy.GetParameterAsText(2)
    buffDist=arcpy.GetParameter(3)
    arcpy.AddMessage(f"buffDist: {buffDist} {type(buffDist)}")
    where_clause = f"{arcpy.AddFieldDelimiters(layer, field_name)} > {threshold}"
    selected_layer = arcpy.SelectLayerByAttribute_management(layer, "NEW_SELECTION", where_clause)
    arcpy.env.workspace = r"C:\Users\gis-t10\Documents\ArcGIS\Projects\hasan"  
    arcpy.CreateFileGDB_management(arcpy.env.workspace, "buffer_GDB.gdb")
    arcpy.AddMessage(arcpy.env.workspace)
    arcpy.AddMessage(buffDist)

    for distance in buffDist:
        output_name = "Buffer_{}".format(distance)
        output_path = arcpy.env.workspace + "\\buffer_GDB.gdb\\" + output_name
        arcpy.Buffer_analysis(selected_layer, output_path, distance)
    arcpy.AddMessage("Buffer analysis completed successfully.")
perform_threshold()
