global function RTKStruct_GetOrCreateEmptyStruct
global function RTKStruct_GetOrCreateScriptStruct
global function RTKStruct_GetOrCreateScriptArrayOfStructs

global function RTKDataModelType_CreateStruct
global function RTKDataModelType_GetOrCreateStruct
global function RTKDataModelType_DestroyStruct
global function RTKDataModelType_GetStruct
global function RTKDataModelType_GetDataPath

global const string RTK_MODELTYPE_COMMON = "common"
global const string RTK_MODELTYPE_MENUS = "menus"
global const string RTK_MODELTYPE_GAMEPLAY = "gameplay"

string function GetFullDataModelPath( string pathToRoot, string propertyName )
{
	return pathToRoot + (pathToRoot == "&" ? "" : ".") +  propertyName
}

rtk_struct function GetOrCreateDataModelStruct( string pathToRoot, string propertyName )
{
	string fullPath = GetFullDataModelPath( pathToRoot, propertyName )
	if ( RTKDataModel_HasDataModel( fullPath ) )
		return RTKDataModel_GetStruct( fullPath )
	else
		return RTKDataModel_CreateEmptyStruct( pathToRoot, propertyName )
	unreachable
}

rtk_struct function RTKStruct_GetOrCreateEmptyStruct( rtk_struct props, string propertyName )
{
	if ( RTKStruct_HasProperty( props, propertyName ) )
		return RTKStruct_GetStruct( props, propertyName )
	else
		return RTKStruct_AddEmptyStructProperty( props, propertyName )
	unreachable
}

rtk_struct function RTKStruct_GetOrCreateScriptStruct( rtk_struct props, string propertyName, string structTypeName )
{
	if ( RTKStruct_HasProperty( props, propertyName ) )
		return RTKStruct_GetStruct( props, propertyName )
	else
		return RTKStruct_AddStructProperty( props, propertyName, structTypeName )
	unreachable
}

rtk_array function RTKStruct_GetOrCreateScriptArrayOfStructs( rtk_struct props, string propertyName, string structTypeName )
{
	if( RTKStruct_HasProperty( props, propertyName ) )
		return RTKStruct_GetArray( props, propertyName )
	else
		return RTKStruct_AddArrayOfStructsProperty( props, propertyName, structTypeName )
	unreachable
}



rtk_struct function RTKDataModelType_CreateStruct( string type = "", string propertyName = "", string structType = "", array< string > parentPropertyNames = [] )
{
	string path = RTKDataModelType_GetDataPath( type, "", true )
	rtk_struct data = GetOrCreateDataModelStruct( "&", RTKDataModelType_GetDataPath( type, "", false ) )

	foreach( string p in parentPropertyNames )
	{
		data = GetOrCreateDataModelStruct( path, p )
		path += "." + p
	}

	if( RTKStruct_HasProperty( data, propertyName ) )
		RTKStruct_RemoveProperty( data, propertyName )

	if( structType == "" )
		return RTKStruct_AddEmptyStructProperty( data, propertyName )
	else
		return RTKStruct_AddStructProperty( data, propertyName, structType )
	unreachable
}

rtk_struct function RTKDataModelType_GetOrCreateStruct( string type = "", string propertyName = "", string structType = "", array< string > parentPropertyNames = [] )
{
	string path = RTKDataModelType_GetDataPath( type, "", true )
	rtk_struct data = GetOrCreateDataModelStruct( "&", RTKDataModelType_GetDataPath( type, "", false ) )

	foreach( string p in parentPropertyNames )
	{
		data = GetOrCreateDataModelStruct( path, p )
		path += "." + p
	}

	if( structType == "" )
		return RTKStruct_GetOrCreateEmptyStruct( data, propertyName )
	else
		return RTKStruct_GetOrCreateScriptStruct( data, propertyName, structType )
	unreachable
}

void function RTKDataModelType_DestroyStruct( string type = "", string propertyName = "", array< string > parentPropertyNames = [] )
{
	string path = RTKDataModelType_GetDataPath( type, "", true )
	rtk_struct data = GetOrCreateDataModelStruct( "&", RTKDataModelType_GetDataPath( type, "", false ) )

	foreach( string p in parentPropertyNames )
	{
		data = GetOrCreateDataModelStruct( path, p )
		path += "." + p
	}

	if( RTKStruct_HasProperty( data, propertyName ) )
		RTKStruct_RemoveProperty( data, propertyName )
}

rtk_struct ornull function RTKDataModelType_GetStruct( string type = "", string propertyName = "", bool includeRoot = true, array< string > parentPropertyNames = [] )
{
	string path = RTKDataModelType_GetDataPath( type, propertyName, includeRoot, parentPropertyNames )
	if ( RTKDataModel_HasDataModel( path ) )
		return RTKDataModel_GetStruct( path )

	return null
}

string function RTKDataModelType_GetDataPath( string type = "", string propertyName = "", bool includeRoot = true, array< string > parentPropertyNames = [] )
{
	string path = ( (includeRoot)? "&" : "" ) + type

	foreach( string p in parentPropertyNames )
		path += "." + p

	if( propertyName != "" )
		path += "." + propertyName

	return path
}
