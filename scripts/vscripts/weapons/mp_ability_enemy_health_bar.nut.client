


global function EnemyHealthBar_Init


global function DisplayEnemyHealthBar




global function ShowHealthRUIWithLOSCheck
struct
{
	bool forceHideEnemyHPBar = false
} file



void function EnemyHealthBar_Init()
{

	file.forceHideEnemyHPBar = GetCurrentPlaylistVarBool( "hud_hide_enemyHPBar", false )
	RegisterSignal( "EnemyHealthBarStart" )

}


void function ShowHealthRUIWithLOSCheck( entity owner, entity victim, float duration )
{
	
	if ( !DisplayEnemyHealthBar() )
		return

	if ( victim.IsCloakedIgnoreFlicker( true ) )
		return

	thread ShowHealthRUIWithLOSCheck_Thread( owner, victim, duration )
}

void function ShowHealthRUIWithLOSCheck_Thread( entity owner, entity victim, float duration )
{
	Assert ( IsNewThread(), "Must be threaded off." )

	if ( !IsValid( victim ) )
		return


		
		
		victim.EndSignal( "PlayerHealthRevealed" )


	victim.Signal( "EnemyHealthBarStart" )
	victim.EndSignal( "EnemyHealthBarStart" )

	victim.EndSignal( "OnDestroy" )
	victim.EndSignal( "OnDeath" )

	float endTime = Time() + duration

	var rui = ReconScan_ShowHudForTarget( owner, victim, eShowHealthbarMode.REQUIRE_LOS_NO_RESTORE, false, $"ui/recon_scan_target.rpak", null )

	OnThreadEnd(
		function() : ( owner, victim )
		{
			ReconScan_RemoveHudForTarget( owner, victim, eShowHealthbarMode.REQUIRE_LOS_NO_RESTORE )
		}
	)

	while ( Time() < endTime )
	{
		if ( IsValid( victim ) && IsValid( owner ) )
		{
			bool phaseShifted = victim.IsPlayer() ? victim.IsPhaseShiftedOrPending() : false

			if ( phaseShifted )
			{
				RuiSetBool( rui, "isVisible", false )
				endTime = Time()
			}

			if ( victim.IsCloakedIgnoreFlicker( true ) )
			{
				RuiSetBool( rui, "isVisible", false )
				endTime = Time()
			}
		}
		WaitFrame()
	}
}



bool function DisplayEnemyHealthBar()
{
	return !file.forceHideEnemyHPBar && GetConVarBool( "hud_setting_showEnemyHealthBar" ) && !(IsPrivateMatch() && GetLocalClientPlayer().IsObserver())
}

      