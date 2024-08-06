global function ExtraShields_Init


global function GetPlayerExtraShields
global function GetPlayerExtraShieldsTier






global const string EXTRA_SHIELDS_DURATION_NETFLOAT = "extra_shields_duration"
global const float EXTRA_SHIELDS_TOTAL_DURATION = 28.0
global const int EXTRA_SHIELD_DECAY_RATE = 2







struct
{
	float totalShieldDuration




	var extraShieldDurationRui

}file


void function ExtraShields_Init()
{
	file.totalShieldDuration = GetCurrentPlaylistVarFloat( "extra_shield_total_shield_duration", EXTRA_SHIELDS_TOTAL_DURATION )

	RegisterNetworkedVariable( EXTRA_SHIELDS_DURATION_NETFLOAT, SNDC_PLAYER_EXCLUSIVE, SNVT_FLOAT_RANGE , file.totalShieldDuration, 0.0, file.totalShieldDuration )


	RegisterNetVarFloatChangeCallback( EXTRA_SHIELDS_DURATION_NETFLOAT, ExtraShields_OnExtraShieldDurationChanged )









}


int function GetPlayerExtraShields( entity player )
{
	return player.GetExtraShieldHealth()
}

int function GetPlayerExtraShieldsTier( entity player )
{
	return player.GetExtraShieldTier()
}

float function GetPlayerExtraShieldsDuration( entity player )
{
	return player.GetPlayerNetFloat( EXTRA_SHIELDS_DURATION_NETFLOAT )
}
























































































void function ExtraShields_OnExtraShieldDurationChanged( entity player, float newDuration )
{
	if ( player != GetLocalViewPlayer() )
		return

	int extraShields = GetPlayerExtraShields( player )
	if( newDuration <= 0.0 || extraShields <= 0 )
	{
		if( file.extraShieldDurationRui != null )
		{
			if ( extraShields <= 0 )
			{
				RuiDestroyIfAlive( file.extraShieldDurationRui )
				file.extraShieldDurationRui = null
			}
			else
			{
				RuiSetInt( file.extraShieldDurationRui, "state", eArcFlashState.DECAY )
			}
		}
		return
	}

	if ( file.extraShieldDurationRui == null )
	{
		file.extraShieldDurationRui = CreateCockpitPostFXRui( $"ui/extra_shield_indicator.rpak", HUD_Z_BASE )
		RuiSetFloat( file.extraShieldDurationRui, "timeTotal", file.totalShieldDuration )
		RuiSetInt( file.extraShieldDurationRui, "state", eArcFlashState.ACTIVE )
		RuiTrackInt( file.extraShieldDurationRui, "tierColor", player, RUI_TRACK_EXTRA_SHIELD_TIER_INT )
	}

	RuiTrackFloat( file.extraShieldDurationRui, "timeRemaining", player, RUI_TRACK_SCRIPT_NETWORK_VAR, GetNetworkedVariableIndex( EXTRA_SHIELDS_DURATION_NETFLOAT ) )
}

void function ExtraShields_OnExtraShieldTierChanged( entity player, int newTier )
{
	if( file.extraShieldDurationRui != null && player == GetLocalViewPlayer())
	{
		vector tierColor = GetKeyColor( COLORID_TEXT_LOOT_TIER0, newTier ) / 255.0
		RuiSetFloat3( file.extraShieldDurationRui, "colorTint", tierColor )
	}
}

