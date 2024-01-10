
global function ThunderdomeSkull_Init



global function ServerToClient_TriggerThunderdomeSkullClientSFX




















const float THUNDERDOME_SKULL_INTERACTION_SMOKE_FX_LIFETIME = 30.0

struct
{




} file

void function ThunderdomeSkull_Init()
{









	Remote_RegisterClientFunction( "ServerToClient_TriggerThunderdomeSkullClientSFX" )
}






































































































































void function TriggerThunderdomeSkullClientSFX()
{
	array<entity> emitterArray = GetEntArrayByScriptName("Thunderdome_skull_smoke_audio_emitter")

	foreach ( entity emitter in emitterArray )
	{
		if ( IsValid( emitter ) )
			emitter.SetEnabled( true )
	}

	wait THUNDERDOME_SKULL_INTERACTION_SMOKE_FX_LIFETIME

	foreach ( entity emitter in emitterArray )
	{
		if ( IsValid( emitter ) )
			emitter.SetEnabled( false )
	}
}



void function ServerToClient_TriggerThunderdomeSkullClientSFX()
{
thread TriggerThunderdomeSkullClientSFX()
}



































