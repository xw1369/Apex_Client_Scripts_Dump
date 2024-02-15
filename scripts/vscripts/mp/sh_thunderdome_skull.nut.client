global function ThunderdomeSkull_Init


global function ServerToClient_ThunderdomeSkullInteractionPlayGroundSmokeSound


#if DEV



#endif







































const float TIME_FX_SMOKE_GROUND_START	= 3.5
const float TIME_FX_SMOKE_GROUND_END	= 33.5



const string SCRIPT_NAME_FIRE_AUDIO		= "Thunderdome_skull_fire_audio_emitter"
const string SCRIPT_NAME_SMOKE_AUDIO	= "Thunderdome_skull_smoke_audio_emitter"


struct
{














	float fx_smoke_ground_start
	float fx_smoke_ground_end
	float fx_smoke_ground_lifetime
} times

struct
{





} file


void function ThunderdomeSkull_Init()
{
	RegisterSignal( "ThunderdomeSkullButtonReset" )


















	Remote_RegisterClientFunction( "ServerToClient_ThunderdomeSkullInteractionPlayGroundSmokeSound" )

	ThunderdomeSkullInteractionInitTimes()
}

void function ThunderdomeSkullInteractionInitTimes()
{














	times.fx_smoke_ground_start = GetCurrentPlaylistVarFloat( "thunderdome_skull_interaction_time_cooldown_button", TIME_FX_SMOKE_GROUND_START )
	times.fx_smoke_ground_end = GetCurrentPlaylistVarFloat( "thunderdome_skull_interaction_time_cooldown_button", TIME_FX_SMOKE_GROUND_END )

	times.fx_smoke_ground_lifetime = times.fx_smoke_ground_end - times.fx_smoke_ground_start
}

















































































































































































































































void function ServerToClient_ThunderdomeSkullInteractionPlayGroundSmokeSound()
{
	thread Client_ThunderdomeSkullInteractionPlayAudioEmitters( SCRIPT_NAME_SMOKE_AUDIO, times.fx_smoke_ground_lifetime )
}



void function Client_ThunderdomeSkullInteractionPlayAudioEmitters( string scriptName, float lifetime )
{
	array<entity> audioEmitters = GetEntArrayByScriptName( scriptName )

	foreach ( entity emitter in audioEmitters )
	{
		if ( IsValid( emitter ) )
		{
			emitter.SetEnabled( true )
		}
	}

	wait lifetime

	foreach ( entity emitter in audioEmitters )
	{
		if ( IsValid( emitter ) )
		{
			emitter.SetEnabled( false )
		}
	}
}


#if DEV









#endif
