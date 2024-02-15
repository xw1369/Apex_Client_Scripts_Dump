global function MpAbilityValkJets_Init

global function OnWeaponActivate_ability_valk_jets
global function OnWeaponDeactivate_ability_valk_jets
global function OnWeaponPrimaryAttack_ability_valk_jets
global function OnWeaponAttemptOffhandSwitch_ability_valk_jets






global function Valk_EnableHudColorCorrection
global function Valk_DisableHudColorCorrection
global function Valk_CreateJetPackRui
global function Valk_GetJetPackRui
global function Valk_DestroyJetPackRui


global function CodeCallback_OnPlayerJetpackStop
global function CodeCallback_OnPlayerJetpackStart

const float SLOW_FALL_TIME = 0.5
const float VALK_JETPACK_SPEED = 250
const float VALK_JETPACK_REACTIVATION_DELAY = 0.25
const asset SKYWARD_JUMPJETS_FRIENDLY = $"P_valk_jet_fly_ON"
const asset SKYWARD_JUMPJETS_ENEMY = $"P_valk_jet_fly_ON"

const asset VALK_AMB_EXHAUST_FP = $"P_valk_spear_thruster_idle"
const asset VALK_AMB_EXHAUST_3P = $"P_valk_spear_thruster_idle_3P"

struct
{

		int  colorCorrection
		bool colorCorrectionActive

		int                       valkTrackersActive
		array<entity>             valkEnemiesTracked
		table<entity, int>        valkEnemiesTrackedCount

		var jetPackRui


	table<entity, float>                  valkToJumpHeldStartTime
	table<entity, array<entity> >         valkToJumpJetFXs







} file


void function MpAbilityValkJets_Init()
{
	PrecacheWeapon( "mp_ability_valk_jets" )

	RegisterSignal( "JetpackPassiveRemoved" )
	RegisterSignal( "JetpackOff" )
	RegisterSignal( "ValkFreefallEnd" )
	RegisterSignal( "ValkFlightReveal" )
	RegisterSignal( "ValkTeammateStartTracking" )

	RegisterNetworkedVariable( "valkTrackingActive", SNDC_PLAYER_GLOBAL, SNVT_BOOL, false )






		RegisterNetVarBoolChangeCallback( "valkTrackingActive", OnValkTrackingChanged )
		AddCallback_CreatePlayerPassiveRui( Valk_CreateJetPackRui )
		AddCallback_DestroyPlayerPassiveRui( Valk_DestroyJetPackRui )
		file.colorCorrection = ColorCorrection_Register( "materials/correction/launch_hud.raw_hdr" )


	AddCallback_OnPassiveChanged( ePassives.PAS_VALK, OnPassiveChanged )

	PrecacheParticleSystem( VALK_AMB_EXHAUST_FP )
	PrecacheParticleSystem( VALK_AMB_EXHAUST_3P )
}


void function ValkTeammateStartTracking( entity valk )
{
	valk.EndSignal( "OnDeath" )
	valk.EndSignal( "OnDestroy" )
	valk.EndSignal( "ValkTeammateStartTracking" )

	file.valkTrackersActive++
	if ( file.valkTrackersActive == 1 )
	{
		thread Thread_ValkFlightReveal()
	}

	array<entity> targetsShown = []

	OnThreadEnd(
		function() : ( valk, targetsShown )
		{
			file.valkTrackersActive--
			if ( file.valkTrackersActive == 0 )
			{
				Signal( clGlobal.levelEnt, "ValkFlightReveal" )
			}

			foreach ( enemy in targetsShown )
			{
				StopValkFlightRevealForTeam( enemy )
			}
		}
	)

	var rui
	
	

	float valkPasRevealDistance = GetCurrentPlaylistVarFloat( "valkpas_enemy_reveal_distance", 69000000 )
	while ( true )
	{
		int valkTeam				= valk.GetTeam()
		array<entity> enemyPlayers 	= GetPlayerArrayOfEnemies( valkTeam )
		array<entity> decoyArray 	= GetPlayerDecoyArray()
		decoyArray.extend( GetEntArrayByScriptName( MIRAGE_DECOY_DROP_SCRIPTNAME ) )

		if ( IsFiringRangeGameMode() )
		{
			array<entity> dummies = GetEntArrayByScriptName( FIRING_RANGE_DUMMIE_SCRIPT_NAME )
			dummies.extend( GetEntArrayByScriptName( FIRING_RANGE_COMBAT_DUMMIE_SCRIPT_NAME ) )

			enemyPlayers.extend( dummies )
		}

		foreach ( decoy in decoyArray )
		{
			if( !IsValid(decoy) )
				continue

			int decoyTeam = decoy.GetTeam()
			if( decoyTeam != valkTeam )
				enemyPlayers.append( decoy )
		}

		foreach ( enemy in enemyPlayers )
		{
			bool dropThisEnemy
			bool isDropDecoy
			string scriptName

			if( IsValid( enemy ) )
			{
				scriptName = enemy.GetScriptName()
				isDropDecoy = ( scriptName == MIRAGE_DECOY_DROP_SCRIPTNAME )
			}

			if ( IsAlive( enemy ) || isDropDecoy )
			{
				dropThisEnemy = false

				if ( DistanceSqr( valk.GetOrigin(), enemy.GetOrigin() ) < valkPasRevealDistance )
				{
					vector enemyTracePos = enemy.GetOrigin()
					if( !isDropDecoy )
						enemyTracePos = enemy.EyePosition()

					TraceResults trace = TraceLine( valk.EyePosition(), enemyTracePos, [ valk ], TRACE_MASK_VISIBLE, TRACE_COLLISION_GROUP_NONE )
					if ( trace.fraction == 1.0 && ValkThreatVisionShouldRevealEnemy( enemy ) )
					{
						if ( !(targetsShown.contains( enemy )) )
						{
							StartValkFlightRevealForTeam( enemy )
							targetsShown.append( enemy )
						}
					}
					else
					{
						
						dropThisEnemy = true
					}
				}
				else
				{
					
					dropThisEnemy = true
				}
			}
			else
			{
				
				dropThisEnemy = true
			}

			if ( dropThisEnemy )
			{
				if ( targetsShown.contains( enemy ) )
				{
					targetsShown.removebyvalue( enemy )
					StopValkFlightRevealForTeam( enemy )
				}
			}

			WaitFrame()
		}

		WaitFrame()
	}
}

void function Valk_CreateJetPackRui( entity player )
{
	if ( PlayerHasPassive( player, ePassives.PAS_VALK ) )
	{
		if ( file.jetPackRui != null )
			return

		file.jetPackRui = CreateCockpitRui( $"ui/valk_jets_meter.rpak" )

		RuiTrackFloat( file.jetPackRui, "chargeFrac", player, RUI_TRACK_GLIDE_METER_FRACTION )
		RuiTrackFloat( file.jetPackRui, "bleedoutEndTime", player, RUI_TRACK_SCRIPT_NETWORK_VAR, GetNetworkedVariableIndex( "bleedoutEndTime" ) )
		RuiTrackFloat( file.jetPackRui, "reviveEndTime", player, RUI_TRACK_SCRIPT_NETWORK_VAR, GetNetworkedVariableIndex( "reviveEndTime" ) )
	}
}

var function Valk_GetJetPackRui()
{
	return file.jetPackRui
}

void function Valk_DestroyJetPackRui( entity player )
{
	if( !PlayerHasPassive( player, ePassives.PAS_VALK ) )
	{
		if ( file.jetPackRui != null )
		{
			RuiDestroyIfAlive( file.jetPackRui )
			file.jetPackRui = null
		}
	}
}


bool function ValkThreatVisionShouldRevealEnemy( entity enemy )
{
	if ( !enemy.IsPlayer() && !enemy.IsPlayerDecoy() )
	{
		string scriptName = enemy.GetScriptName()
		if( scriptName == MIRAGE_DECOY_DROP_SCRIPTNAME )
			return true
		else if ( IsFiringRangeGameMode() && ( scriptName == FIRING_RANGE_DUMMIE_SCRIPT_NAME || scriptName == FIRING_RANGE_COMBAT_DUMMIE_SCRIPT_NAME ) )
			return true
		else
			return false
	}

	if( !enemy.IsPlayerDecoy() )
	{
		if ( BleedoutState_GetPlayerBleedoutState( enemy ) != BS_NOT_BLEEDING_OUT )
			return false
	}






	return true
}

void function StartValkFlightRevealForTeam( entity enemy )
{
	array<entity> trackedEnemies = file.valkEnemiesTracked

	if ( ! (enemy in file.valkEnemiesTrackedCount) )
	{
		file.valkEnemiesTrackedCount[ enemy ] <- 0
	}

	file.valkEnemiesTrackedCount[ enemy ] += 1

	if ( file.valkEnemiesTrackedCount[ enemy ] == 1 )
		file.valkEnemiesTracked.append( enemy )
}

void function StopValkFlightRevealForTeam( entity enemy )
{
	file.valkEnemiesTrackedCount[ enemy ] -= 1

	if ( file.valkEnemiesTrackedCount[ enemy ] == 0 )
	{
		file.valkEnemiesTracked.fastremovebyvalue( enemy )
	}
}

void function Thread_ValkFlightReveal()
{
	entity player = GetLocalViewPlayer()
	Signal( clGlobal.levelEnt, "ValkFlightReveal" )
	EndSignal( clGlobal.levelEnt, "ValkFlightReveal" )
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "OnDeath" )

	array<entity> activeEnts

	while ( true )
	{
		foreach ( ent in file.valkEnemiesTracked )
		{
			if ( !activeEnts.contains( ent ) )
			{
				activeEnts.append( ent )
				thread _ValkFlightReveal( ent )
			}
		}

		foreach ( ent in clone activeEnts )
		{
			if ( !file.valkEnemiesTracked.contains( ent ) )
			{
				Signal( ent, "ValkFlightReveal" )
				activeEnts.fastremovebyvalue( ent )
			}
		}

		wait 0.5
	}
}

void function _ValkFlightReveal( entity victim )
{
	if ( !IsValid( victim ) )
		return

	Signal( victim, "ValkFlightReveal" )

	EndSignal( clGlobal.levelEnt, "ValkFlightReveal" )
	entity player = GetLocalViewPlayer()
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "ValkFreefallEnd" )
	EndSignal( victim, "ValkFlightReveal" )
	EndSignal( victim, "OnDestroy" )
	var rui = RuiCreate( $"ui/recon_overview_scan_target.rpak", clGlobal.topoFullScreen, RUI_DRAW_HUD, 0 )
	InitHUDRui( rui )

	EmitSoundOnEntity( GetLocalViewPlayer(), "Valk_Ultimate_AcquireTarget_1P" )

	RuiSetBool( rui, "isVisible", true )
	RuiSetBool( rui, "pinToEdge", true )
	RuiSetBool( rui, "showClampArrow", true )
	RuiSetString( rui, "hint", "" )

	int attachment = victim.LookupAttachment( "CHESTFOCUS" )
	RuiTrackFloat3( rui, "pos", victim, RUI_TRACK_POINT_FOLLOW, attachment )
	bool isChampion   = IsFiringRangeGameMode() && victim.IsNPC() ? false : GradeFlagsHas( victim, eTargetGrade.CHAMPION )
	bool isKillLeader = IsFiringRangeGameMode() && victim.IsNPC() ? false : GradeFlagsHas( victim, eTargetGrade.CHAMP_KILLLEADER )
	RuiSetBool( rui, "isChampion", isChampion )
	RuiSetBool( rui, "isKillLeader", isKillLeader )


		if ( Control_IsModeEnabled() )
		{
			bool isEXPLeader = GradeFlagsHas( victim, eTargetGrade.EXP_LEADER )
			RuiSetBool( rui, "isEXPLeader", isEXPLeader )
		}



	var fRui = FullMap_AddEnemyLocation( victim )
	var mRui = Minimap_AddEnemyToMinimap( victim )

	OnThreadEnd (
		function() : ( victim, rui, fRui, mRui )
		{
			RuiDestroy( rui )
			Fullmap_RemoveRui( fRui )
			RuiDestroy( fRui )
			Minimap_CommonCleanup( mRui )
		}
	)








		WaitForever()

}

void function OnValkTrackingChanged( entity player, bool new )
{
	if ( !IsValid( GetLocalViewPlayer() ) )
		return

	if ( player.GetTeam() != GetLocalViewPlayer().GetTeam() )
	{
		
		if ( !AllianceProximity_ShouldTryToTransmitPingOrIconToAlliance( false ) || !IsValid( player ) || !IsFriendlyTeam( player.GetTeam(), GetLocalViewPlayer().GetTeam() ) || !IsPositionWithinRadius( AllianceProximity_GetMaxDistForProximity(), GetLocalViewPlayer().GetOrigin(), player.GetOrigin() ) )
			return
	}

	if ( new )
	{
		thread ValkTeammateStartTracking( player )
	}
	else
	{
		player.Signal( "ValkTeammateStartTracking" )
	}
}

void function Valk_EnableHudColorCorrection()
{
	
	if ( !file.colorCorrectionActive )
	{
		ColorCorrection_SetExclusive( file.colorCorrection, true )
		file.colorCorrectionActive = true
		for ( float intensity = 0.0; intensity <= 1.0; intensity += 0.05 )
		{
			ColorCorrection_SetWeight( file.colorCorrection, intensity )
			Wait( 0.05 )
		}
	}
}

void function Valk_DisableHudColorCorrection()
{
	
	if ( file.colorCorrectionActive )
	{
		file.colorCorrectionActive = false
		for ( float intensity = 1.0; intensity >= 0.0; intensity -= 0.05 )
		{
			ColorCorrection_SetWeight( file.colorCorrection, intensity )
			Wait( 0.05 )
		}
		ColorCorrection_SetExclusive( file.colorCorrection, false )
	}
	OnThreadEnd(
		function()
		{
			
		}
	)
}


















































































bool function OnWeaponAttemptOffhandSwitch_ability_valk_jets( entity weapon )
{
	entity weaponOwner = weapon.GetWeaponOwner()
	entity lastWpn     = weaponOwner.GetLatestPrimaryWeapon( eActiveInventorySlot.mainHand )
	if ( !IsValid( lastWpn ) )
		return false


	entity primaryMelee = weaponOwner.GetNormalWeapon( WEAPON_INVENTORY_SLOT_PRIMARY_2 )
	if( IsValid( primaryMelee ) && primaryMelee == lastWpn && primaryMelee.GetWeaponSettingBool( eWeaponVar.is_heirloom ) && !primaryMelee.GetWeaponSettingBool( eWeaponVar.is_artifact ) )
		primaryMelee.AddMod( "using_jets" )


	if ( weaponOwner.GetPlayerNetBool( "isHealing" ) )
		return false

	float now = Time()
	if ( now < weaponOwner.p.lastTimeDeactivatedJetpack + VALK_JETPACK_REACTIVATION_DELAY )
		return false

	int result = weaponOwner.CanUseJetpack( weaponOwner.GetVelocity() )

		if ( result == JETPACK_ENGAGE_FAILED_OUT_OF_FUEL )
		{
			EmitSoundOnEntity( weaponOwner, "Valk_Hover_Start_Fail_1P" )
		}


	return result == JETPACK_ENGAGE_SUCCEED
}

var function OnWeaponPrimaryAttack_ability_valk_jets( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	return 0
}

void function OnWeaponActivate_ability_valk_jets( entity weapon )
{
	entity owner = weapon.GetWeaponOwner()

	owner.SetActivateJetpack( true )
	owner.Signal("JumpPadStart") 


		if ( GetLocalViewPlayer() != owner )
			return

		ClientScreenShake( 5, 12, 0.3, <0, 0, 1> )










		if ( GetLocalViewPlayer() == owner )
		{
			
		}


	if ( weapon.HasMod( "heirloom" ) )
	{
		weapon.PlayWeaponEffect( VALK_AMB_EXHAUST_FP, VALK_AMB_EXHAUST_3P, "fx_l_thruster_top" , true )
		weapon.PlayWeaponEffect( VALK_AMB_EXHAUST_FP, VALK_AMB_EXHAUST_3P, "fx_l_thruster_bot" , true )
		weapon.PlayWeaponEffect( VALK_AMB_EXHAUST_FP, VALK_AMB_EXHAUST_3P, "fx_r_thruster_top" , true )
		weapon.PlayWeaponEffect( VALK_AMB_EXHAUST_FP, VALK_AMB_EXHAUST_3P, "fx_r_thruster_bot" , true )
	}
}


























void function OnWeaponDeactivate_ability_valk_jets( entity weapon )
{
	entity valk = weapon.GetWeaponOwner() 
	if ( !IsValid( valk ) )
		return

	valk.Signal( "JetpackOff" )
	valk.SetActivateJetpack( false )
	valk.p.lastTimeDeactivatedJetpack = Time()



}

void function OnPassiveChanged( entity player, int passive, bool didHave, bool nowHas )
{
	if ( didHave )
	{







	}
	if ( nowHas )
	{

			if ( player == GetLocalClientPlayer() )
				UpdateAbilityToggleOrHoldBasedOnInput()




	}
}



void function CodeCallback_OnPlayerJetpackStop( entity player )
{
	player.SetActivateJetpack( false )















}


void function CodeCallback_OnPlayerJetpackStart( entity player )
{













}












































































