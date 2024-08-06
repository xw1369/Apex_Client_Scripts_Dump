global function Perk_RingExpert_Init
global function Perk_RingExpert_IsEnabled




global function Perk_RingExpert_MonitorEnemyPresence


global const string RING_EXPERT_END_SIGNAL = "ring_expert_end"
const string RING_DIRECTION_CREATED_SIGNAL = "ring_direction_created"
const string RING_DIRECTION_SCRIPT_NAME = "ring_direction"
const string OVERSHIELD_FULLY_DRAINED = "Controller_Overcharge_Empty_1p"
const asset DAMAGE_MITIGATION_1P_SCREEN_FX = $"P_clbr_ulti_screen_colored"
const asset DAMAGE_MITIGATION_FLASH_FX = $"P_cPerks_con_shield_up"
const asset DAMAGE_MITIGATION_END_FX = $"P_cPerks_con_shield_down"
const string OVERSHIELD_START_SOUND = "Controller_Zone_Enter_1p"
const string OVERSHIELD_END_SOUND = "Controller_Zone_Exit_1p"
const string OVERSHIELD_FULLY_CHARGED = "Controller_Overcharge_Full_1p"

const asset RING_RADIUS_FX = $"P_ar_edge_ring_gen"
const vector RING_RADIUS_COLOR = <60, 110, 300>
const float AR_EFFECT_SIZE = 768.0 

int COCKPIT_DAMAGE_MITIGATION_SCREEN_FX
int DAMAMGE_MITIGATION_START_FX
int DAMAMGE_MITIGATION_END_FX

struct {








	bool  enemiesDetectedInPath = false
	bool  enemiesDetectedInRing = false
	bool monitorEnemies = false
	entity ringFxEntity = null
	var damageMitigationRui = null

}file

struct {
	float evoTickRate = 1
	float nextRingDirScanWidth = 3000
	int zoneEnterShieldRegen = 25
	float nextZoneMonitorTime = 60
	bool nextZoneMonitorEnemyPresence = false
	bool displayNextRingCompass = false
	float shieldDecayTickRate = .1
	float shieldDecayDelay = 5.0
	float shieldChargeTickRate = .1
	float regenCooldown = 0
	bool rechargeOnRingEnterWhenFull = true
}tunings

void function Perk_RingExpert_Init()
{
	PerkInfo ringExpert

	ringExpert.activateCallback = Perk_RingExpert_OnActivate
	ringExpert.deactivateCallback = Perk_RingExpert_OnDeactivate

	ringExpert.perkId          = ePerkIndex.RING_EXPERT
	Perks_RegisterClassPerk( ringExpert )


	RegisterSignal( RING_EXPERT_END_SIGNAL )
	RegisterSignal( RING_DIRECTION_CREATED_SIGNAL )
	PrecacheScriptString( RING_DIRECTION_SCRIPT_NAME )


	RegisterSignal( "Perk_StructureDamageMitigation_StatusStarted" )
	RegisterSignal( "Perk_StructureDamageMitigation_StatusEnded" )

	if( !Perk_RingExpert_IsEnabled() )
		return


		COCKPIT_DAMAGE_MITIGATION_SCREEN_FX = PrecacheParticleSystem( DAMAGE_MITIGATION_1P_SCREEN_FX )
		DAMAMGE_MITIGATION_START_FX = PrecacheParticleSystem( DAMAGE_MITIGATION_FLASH_FX )
		DAMAMGE_MITIGATION_END_FX = PrecacheParticleSystem( DAMAGE_MITIGATION_END_FX )

		StatusEffect_RegisterEnabledCallback( eStatusEffect.structure_damage_mitigation, DamageMitigationScreenVFXEnabled )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.structure_damage_mitigation, DamageMitigationScreenVFXDisabled )

		AddCreateCallback( "prop_care_package_insight", Perk_RingExpert_RingDirectionCreated )

		
		

}

void function Perk_RingExpert_InitTunings()
{
	tunings.evoTickRate = GetCurrentPlaylistVarFloat( "perk_ring_expert_evo_tick_rate", tunings.evoTickRate )
	tunings.zoneEnterShieldRegen = GetCurrentPlaylistVarInt( "perk_ring_expert_zone_enter_shield_regen", tunings.zoneEnterShieldRegen )
	tunings.nextZoneMonitorEnemyPresence = GetCurrentPlaylistVarBool( "perk_ring_expert_next_zone_monitor_enemy_presence", tunings.nextZoneMonitorEnemyPresence )
	tunings.nextRingDirScanWidth = GetCurrentPlaylistVarFloat( "perk_ring_expert_next_ring_dir_scan_width", tunings.nextRingDirScanWidth )
	tunings.nextZoneMonitorTime = GetCurrentPlaylistVarFloat( "perk_ring_expert_next_ring_dir_monitor_time", tunings.nextZoneMonitorTime )
	tunings.shieldDecayTickRate = GetCurrentPlaylistVarFloat( "perk_overshield_decay_tick_rate", tunings.shieldDecayTickRate )
	tunings.shieldChargeTickRate = GetCurrentPlaylistVarFloat( "perk_overshield_charge_tick_rate", tunings.shieldChargeTickRate )
	tunings.shieldDecayDelay = GetCurrentPlaylistVarFloat( "perk_overshield_decay_delay", tunings.shieldDecayDelay )
	tunings.regenCooldown = GetCurrentPlaylistVarFloat( "perk_overshield_charge_cooldown", tunings.regenCooldown )
	tunings.rechargeOnRingEnterWhenFull = GetCurrentPlaylistVarBool( "perk_overshield_charge_when_full", tunings.rechargeOnRingEnterWhenFull )
}

bool function Perk_RingExpert_IsEnabled()
{
	return Perks_S22UpdateEnabled() && GetCurrentPlaylistVarBool( "perk_ring_expert_is_enabled", true )
}


void function Perk_RingExpert_OnActivate( entity player, string characterName )
{
	player.Signal( RING_EXPERT_END_SIGNAL )




	if( player == GetLocalViewPlayer() )
	{
		thread Perk_QuickPackup_NagThread( player )
	}

}

void function Perk_RingExpert_OnDeactivate( entity player )
{
	player.Signal( RING_EXPERT_END_SIGNAL )
}
































































































































































































































void function Perk_RingExpert_MonitorEnemyPresence( entity ent, var rui, bool drawLine )
{
	if( !Perks_S22UpdateEnabled() )
		return

	if( !tunings.nextZoneMonitorEnemyPresence )
		return

	int nextDeathfieldStage = minint( SURVIVAL_GetCurrentDeathFieldStage() + 1, Survival_GetNumDeathfieldStages() )
	DeathFieldStageData stageData = Cl_GetDeathFieldStage( nextDeathfieldStage )
	float stageRadius = stageData.endRadius
	entity localPlayer = GetLocalViewPlayer()
	if( !IsValid( localPlayer ) )
		return
	int localTeam = localPlayer.GetTeam()
	vector fieldOrigin = ent.GetOrigin()

	if( drawLine )
	{
		RuiSetBool( rui, "drawToBorder", true )
		RuiSetBool( rui, "drawLine", true )
		RuiTrackFloat3( rui, "playerPos", GetLocalViewPlayer(), RUI_TRACK_ABSORIGIN_FOLLOW )
		RuiSetFloat( rui, "worldLineWidth", tunings.nextRingDirScanWidth )
	}
	ent.EndSignal( "OnDestroy" )

	OnThreadEnd(
		function() : ()
		{
			file.monitorEnemies = false
		}
	)

	file.monitorEnemies = true

	while( true )
	{
		if( file.enemiesDetectedInPath )
		{
			RuiSetFloat3( rui, "lineColor", SrgbToLinear( TEAM_COLOR_ENEMY / 255.0 ) )  
		}
		else
		{
			RuiSetFloat3( rui, "lineColor", SrgbToLinear( TEAM_COLOR_PARTY / 255.0 ) )  
		}
		if( file.enemiesDetectedInRing )
		{
			RuiSetColorAlpha( rui, "objColor", SrgbToLinear( TEAM_COLOR_ENEMY / 255.0 ), SAFE_ZONE_ALPHA )  
		}
		else
		{
			RuiSetColorAlpha( rui, "objColor", SrgbToLinear( TEAM_COLOR_PARTY / 255.0 ), SAFE_ZONE_ALPHA )  
		}
		Wait( .5 )
	}
}

void function Perk_RingExpert_RingDirectionCreated( entity ent )
{
	if( ent.GetScriptName() != RING_DIRECTION_SCRIPT_NAME )
		return

	thread Perk_RingExpert_RingDirectionLifetime_Thread( ent )
}

void function Perk_RingExpert_RingDirectionLifetime_Thread( entity ent )
{
	entity localPlayer = GetLocalViewPlayer()
	if( !Perks_DoesPlayerHavePerk( localPlayer, ePerkIndex.RING_EXPERT ) )
		return

	clGlobal.levelEnt.Signal( RING_DIRECTION_CREATED_SIGNAL )
	clGlobal.levelEnt.EndSignal( RING_DIRECTION_CREATED_SIGNAL )
	ent.EndSignal( "OnDestroy" )

	var compassRui = GetCompassRui()
	if( compassRui == null )
		return

	OnThreadEnd(
		function() : ( compassRui )
		{
			RuiSetBool( compassRui, "showRingIcon", false )
		}
	)

	localPlayer.EndSignal( "OnDestroy", "OnDeath" )

	int team = localPlayer.GetTeam()
	vector nextRingOrigin = ent.GetOrigin()
	RuiSetBool( compassRui, "showRingIcon", true )
	
	while( Perks_DoesPlayerHavePerk( localPlayer, ePerkIndex.RING_EXPERT ) )
	{
		bool inZone = Cl_PosInSafeZone( localPlayer.GetOrigin() )

		vector ringOrigin = inZone ? nextRingOrigin : Cl_SURVIVAL_GetSafeZoneCenter()
		RuiSetFloat( compassRui, "ringAngle", GetCompassAngleFromPosition( ringOrigin ) )
		RuiSetBool( compassRui, "showingNextRing", inZone )

		if( tunings.nextZoneMonitorEnemyPresence )
		{
			entity surveyZone = GetActiveSurveyZone( localPlayer )
			if( file.monitorEnemies && IsValid( surveyZone ) )
			{
				file.enemiesDetectedInPath = false
				array<entity> aliveEnemies = GetPlayerArrayOfEnemies_Alive( team )
				if( aliveEnemies.len() > 0 )
				{
					int nextDeathfieldStage = minint( SURVIVAL_GetCurrentDeathFieldStage() + 1, Survival_GetNumDeathfieldStages() )
					DeathFieldStageData stageData = Cl_GetDeathFieldStage( nextDeathfieldStage )
					float stageRadius = stageData.endRadius
					vector playerOrigin = localPlayer.GetOrigin()
					vector dirToRing = ( surveyZone.GetOrigin() - localPlayer.GetOrigin() )
					dirToRing.z = 0
					float distToRing = Length( dirToRing )
					dirToRing /= distToRing
					float detectionDist = distToRing - stageRadius
					if( detectionDist > 0 )
					{
						array<entity> nearbyEnemies = GetEntitiesFromArrayNearPos( aliveEnemies, playerOrigin, detectionDist + tunings.nextRingDirScanWidth )
						foreach( entity enemy in nearbyEnemies )
						{
							vector enemyPos = enemy.GetOrigin()
							vector enemyDelta = enemyPos - playerOrigin
							enemyDelta.z = 0
							float dotProduct = DotProduct( dirToRing, enemyDelta )
							if( dotProduct < 0 )
								continue
							vector pointAlongLookDir = dotProduct * dirToRing
							float distSqrFromPoint = Distance2DSqr( enemyPos, playerOrigin + pointAlongLookDir )
							if( distSqrFromPoint > tunings.nextRingDirScanWidth * tunings.nextRingDirScanWidth )
								continue

							file.enemiesDetectedInPath = true
						}
					}

					array<entity> nextRingEnemies = GetEntitiesFromArrayNearPos( aliveEnemies, Cl_SURVIVAL_GetSafeZoneCenter(), stageRadius )
					file.enemiesDetectedInRing = nextRingEnemies.len() > 0
				}
				if( file.enemiesDetectedInPath || file.enemiesDetectedInRing )
				{
					RuiSetFloat3( compassRui, "ringDirectionColor", SrgbToLinear( <1.0, 0, 0> ) )
				}
				else
				{
					RuiSetFloat3( compassRui, "ringDirectionColor", SrgbToLinear( TEAM_COLOR_PARTY / 255.0 ) )
				}
			}
			else
			{
				RuiSetFloat3( compassRui, "ringDirectionColor", <1, 0.5, 0> )
			}
		}

		WaitFrame()
	}

}




bool function Cl_PosInSafeZone( vector position )
{
	int i = SURVIVAL_GetCurrentDeathFieldStage()

	if ( i == -1 )
		return true

	DeathFieldStageData data = Cl_GetDeathFieldStage( i )
	float endRadius = data.endRadius
	vector deathFieldCenter = Cl_SURVIVAL_GetSafeZoneCenter()

	float distanceFromCenter = Distance2D( position, deathFieldCenter )
	return distanceFromCenter < endRadius
}

float function GetCompassAngleFromPosition( vector pos )
{
	entity localPlayer = GetLocalViewPlayer()
	vector pingedPlayerDisplacement = FlattenNormalizeVec( pos - localPlayer.GetOrigin() )
	float dot = DotProduct( pingedPlayerDisplacement, < 1, 0, 0 > )
	float result = DotToAngle( dot )
	if ( CrossProduct( pingedPlayerDisplacement, < 1, 0, 0 > ).z < 0 )
	{
		result = -1 * result
	}

	return result
}

void function Perk_RingExpert_InitializeFirstRing()
{
	Perk_RingExpert_RingClosed( SURVIVAL_GetCurrentDeathFieldStage(), -1 )
}

void function Perk_RingExpert_RingClosed( int stage, float nextCloseTime )
{
	if( stage < 0 )
		return

	if( !Perks_DoesPlayerHavePerk( GetLocalViewPlayer(), ePerkIndex.RING_EXPERT ) )
		return

	if( file.ringFxEntity != null )
	{
		file.ringFxEntity.Destroy()
	}
	int nextDeathfieldStage = minint( stage, Survival_GetNumDeathfieldStages() )
	DeathFieldStageData stageData = Cl_GetDeathFieldStage( nextDeathfieldStage )
	file.ringFxEntity =  CreateClientSidePropDynamic( Cl_SURVIVAL_GetSafeZoneCenter(), <0,0,0>, $"mdl/dev/empty_model.rmdl" )
	int debugReminderSphereFXID  = GetParticleSystemIndex( RING_RADIUS_FX )
	int handle = StartParticleEffectOnEntity( file.ringFxEntity, debugReminderSphereFXID, FX_PATTACH_POINT_FOLLOW, file.ringFxEntity.LookupAttachment( "ref" ) )
	float radius = stageData.endRadius / AR_EFFECT_SIZE
	EffectSetControlPointVector( handle, 1, <radius, radius, radius> )
	EffectSetControlPointVector( handle, 2, RING_RADIUS_COLOR )
}

void function DamageMitigationScreenVFXEnabled( entity ent, int statusEffect, bool actuallyChanged )
{
	entity viewPlayer = GetLocalViewPlayer()

	if ( !actuallyChanged && viewPlayer == GetLocalClientPlayer() )
		return

	if ( ent != viewPlayer )
		return

	thread DamageMitigation_1PFX_Thread( viewPlayer )
}

void function DamageMitigationScreenVFXDisabled( entity ent, int statusEffect, bool actuallyChanged )
{
	entity viewPlayer = GetLocalViewPlayer()
	if( viewPlayer != GetLocalClientPlayer() || ent != viewPlayer )
		return

	ent.Signal( "Perk_StructureDamageMitigation_StatusEnded" )
}

void function DamageMitigation_1PFX_Thread( entity player )
{
	player.EndSignal( "OnDeath", "OnDestroy", "BleedOut_OnStartDying" )

	




	if( file.damageMitigationRui != null )
	{
		RuiDestroyIfAlive( file.damageMitigationRui )
	}

	file.damageMitigationRui = CreateCockpitPostFXRui( $"ui/structure_damage_mitigation_indicator.rpak", HUD_Z_BASE )
	RuiTrackInt( file.damageMitigationRui, "extraShieldAmount", player, RUI_TRACK_EXTRA_SHIELD_INT )
	RuiTrackInt( file.damageMitigationRui, "extraShieldTier", player, RUI_TRACK_EXTRA_SHIELD_TIER_INT )

	OnThreadEnd(
		function() : ( player )
		{

			if( file.damageMitigationRui != null)
			{
				RuiDestroyIfAlive( file.damageMitigationRui )
				file.damageMitigationRui = null
			}
		}
	)

	float prevSeverity = 0
	float changeThreshold = .95
	while( true )
	{
		








		float severity = StatusEffect_GetSeverity( player, eStatusEffect.structure_damage_mitigation )

		if( prevSeverity < changeThreshold && severity >= changeThreshold )
		{
			EmitSoundOnEntity( player, OVERSHIELD_START_SOUND )
			PlayShieldOverchargeStatusChangeFx( player, DAMAMGE_MITIGATION_START_FX )
		}
		else if( prevSeverity >= changeThreshold && severity < changeThreshold)
		{
			EmitSoundOnEntity( player, OVERSHIELD_END_SOUND )
			PlayShieldOverchargeStatusChangeFx( player, DAMAMGE_MITIGATION_END_FX )
		}
		prevSeverity = severity

		if( severity == 0 && player.GetExtraShieldHealth() <= 0 )
			break

		RuiSetFloat( file.damageMitigationRui, "fill", severity )

		
		
		WaitFrame()
	}
}

void function PlayShieldOverchargeStatusChangeFx( entity player, int particleSystemIdx )
{
	entity cockpit = player.GetCockpit()
	if ( !cockpit )
		return

	int playerLevel = UpgradeCore_GetPlayerLevel( player )
	if( UpgradeCore_IsLevelMaxLevel( playerLevel ) )
	{
		return
	}

	int fxHandle = StartParticleEffectOnEntity( cockpit, particleSystemIdx, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )

	int armorTier = UpgradeCore_GetArmorTierForLevel( playerLevel + 1 )
	vector shieldColor = GetFXRarityColorForTier( armorTier )

	EffectSetIsWithCockpit( fxHandle, true )
	EffectSetControlPointVector( fxHandle, 1, shieldColor )
}

