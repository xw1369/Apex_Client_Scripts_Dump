global function DividedMoonStoryEvents_Init















struct
{







} file





















void function DividedMoonStoryEvents_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )














}

void function EntitiesDidLoad()
{






			SetupP3Audio()


}




void function SetupP3Audio()
{
	int phase = GetTelTeasePhase()
	if ( phase == eTeleportPhase.PHASE_3 )
	{
		array<entity> alarmEntities = GetEntArrayByScriptName( "StasisArray_AlarmSFX" )
		foreach ( alarmEntity in alarmEntities )
			alarmEntity.SetEnabled( true )

		array<entity> statisArrayPOIArr = GetEntArrayByScriptName( "StasisArray_POI" )
		if ( statisArrayPOIArr.len() == 1 )
			statisArrayPOIArr[0].SetEnabled( false )

		array<entity> statisLaserArr = GetEntArrayByScriptName( "StasisArray_Laser" )
		if ( statisLaserArr.len() == 1 )
			statisLaserArr[0].SetEnabled( false )
	}
}




































































