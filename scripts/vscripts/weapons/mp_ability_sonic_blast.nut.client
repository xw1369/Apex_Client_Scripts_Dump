global function OnWeaponActivate_ability_sonic_blast
global function OnWeaponDeactivate_ability_sonic_blast
global function OnWeaponTossReleaseAnimEvent_weapon_sonic_blast
global function OnWeaponAttemptOffhandSwitch_weapon_sonic_blast
global function OnWeaponTossPrep_weapon_sonic_blast
global function OnWeaponTossCancel_weapon_sonic_blast
global function MpAbilitySonicBlast_Init
global function GetSonicBlastSilenceDuration
global function GetSonicBlastRange
global function GetSonicBlastDoesSonarScan
global function SonicBlast_TargetEntityShouldBeHighlighted

const asset SONIC_BLAST_FX_IMPACT = $"P_wpn_foa_kickup_dust"
const asset SONIC_BLAST_FX_CAST_FP = $"P_wpn_foa_cast_FP"
const asset SONIC_BLAST_FX_WARNING_BEAM = $"P_wpn_foa_beam_warning"
const asset SONIC_BLAST_FX_WARNING_RADIUS = $"P_wpn_foa_radius_MDL_start"
const asset SONIC_BLAST_FX_FRIENDLY_RADIUS = $"P_wpn_foa_radius_MDL_start_friend"
const asset SONIC_BLAST_FX_ENEMY_RADIUS = $"P_wpn_foa_radius_MDL_start_enemy"
const asset SONIC_BLAST_FX_RADIUS = $"P_wpn_foa_radius_MDL"
const asset SONIC_BLAST_FX_HOLD_1P = $"P_wpn_foa_blast_hold"
const asset SONIC_BLAST_FX_HOLD_3P = $"P_wpn_foa_blast_hold_3p"
const asset SONIC_BLAST_FX_TRACER = $"P_foa_warning_mover_spiral"
const asset FX_DRONE_TARGET = $"P_ar_foa_lockon"

const string SONIC_BLAST_MOVER_SCRIPTNAME = "seer_tactical_mover"


global function ServerCallback_ApplyScreenShake
global function ServerCallback_DoDamageIndicator
global function ServerToClient_ShowHealthRUI
global function ServerToClient_SpawnedSonicBlast


global function ShouldBlastHitVictim
global const float SONIC_BLAST_RADIUS = 200.0
global const float SONIC_BLAST_IN_FRONT_START_DISTANCE = 20.0
global const float SONIC_BLAST_RANGE_EXTENSION = 10.0 / INCHES_TO_METERS
const float SONAR_DURATION = 2.5
const float SILENCE_DURATION = 8.0
const float SLOW_DURATION = 0.5 

const float SONIC_BLAST_PROJECTILE_TRAVEL_TIME = 0.5
const float SONIC_BLAST_DEBUG_DRAW_SPHERE_DURATION = 3.75

const int SONIC_BLAST_DAMAGE_AMOUNT = 5
const int SONIC_BLAST_RADIUS_FX_SPACING = 200



#if DEV
const bool SONIC_BLAST_DEBUG = false
const bool SONIC_BLAST_AUDIO_DEBUG = false
#endif

global const string SONIC_BLAST_THREAT_TARGETNAME = "sonic_blast_threat"
const string SONIC_BLAST_MOVESPEED_MOD_NAME = "seer_tac_movespeed_modifier"
const string SONIC_BLAST_3P = "Seer_Tac_Detonate_3p"  
const string SONIC_INITIAL_BLAST_1P = "Seer_Tac_Projectile_3p"  
const string SONIC_INITIAL_BLAST_3P = "Seer_Tac_Projectile_3p" 
const string SONIC_BLAST_INITIAL_CHARGE_1P = "Seer_Tac_Deploy_1p"  
const string SONIC_BLAST_INITIAL_CHARGE_3P = "Seer_Tac_Deploy_3p"  
const string SONIC_BLAST_DISORIENT_1P = "Seer_Tac_Tinnitus_Loop_1p"
const string SONIC_BLAST_SECOND_CHARGE_1P = "Seer_Tac_Shot_1p"  
const string SONIC_BLAST_SECOND_CHARGE_3P = "Seer_Tac_Shot_3p"  
const string SONIC_BLAST_CYLINDER_FORM_3P = "Seer_Tac_Cylinder_Form_3p"  
const string SONIC_BLAST_TARGET_ACQUIRED_SOUND = "Seer_AcquireTarget_1P"

struct findBestSoundPositionData
{
	float fraction
	vector position
}

struct
{




	float sonicBlastRange
	float sonicBlastRangeSqr
	float sonicBlastRadius
	float sonicBlastRadiusSqr
	float sonicBlastSilenceDuration
	float sonicBlastSonarDuration
	float sonicBlastTubeLength
	bool sonicBlastDoesDamage
	bool sonicBlastInterrupts
	bool sonicBlastDoesFullSonarScan
	int sonicBlastDamage

	bool heartbeatSensorActive

} file

void function MpAbilitySonicBlast_Init()
{
	PrecacheParticleSystem( SONIC_BLAST_FX_IMPACT )
	PrecacheParticleSystem( SONIC_BLAST_FX_CAST_FP )
	PrecacheParticleSystem( SONIC_BLAST_FX_WARNING_BEAM )
	PrecacheParticleSystem( SONIC_BLAST_FX_RADIUS )
	PrecacheParticleSystem( SONIC_BLAST_FX_WARNING_RADIUS )
	PrecacheParticleSystem( SONIC_BLAST_FX_HOLD_1P )
	PrecacheParticleSystem( SONIC_BLAST_FX_HOLD_3P )
	PrecacheParticleSystem( SONIC_BLAST_FX_TRACER )
	PrecacheParticleSystem( SONIC_BLAST_FX_FRIENDLY_RADIUS )
	PrecacheParticleSystem( SONIC_BLAST_FX_ENEMY_RADIUS )
	PrecacheParticleSystem( FX_DRONE_TARGET )

	PrecacheScriptString( SONIC_BLAST_THREAT_TARGETNAME )
	PrecacheScriptString( SONIC_BLAST_MOVER_SCRIPTNAME )

	file.sonicBlastRange = HEARTBEAT_SENSOR_NATURAL_RANGE / INCHES_TO_METERS + SONIC_BLAST_RANGE_EXTENSION
	file.sonicBlastRangeSqr = pow( file.sonicBlastRange, 2 )
	file.sonicBlastRadius = GetSonicBlastRadius()
	file.sonicBlastRadiusSqr = pow( file.sonicBlastRadius, 2 )
	file.sonicBlastSilenceDuration = GetSonicBlastSilenceDuration()
	file.sonicBlastSonarDuration = GetSonicBlastSonarDuration()
	file.sonicBlastDoesDamage = GetSonicBlastDoesDamage()
	file.sonicBlastInterrupts = GetSonicBlastInterrupts()
	file.sonicBlastTubeLength = file.sonicBlastRadius * 4.175 
	file.sonicBlastDamage = GetSonicBlastDamage()
	file.sonicBlastDoesFullSonarScan = GetSonicBlastDoesSonarScan()


		StatusEffect_RegisterEnabledCallback( eStatusEffect.seer_highlight_target, SonarBlast_StartHighlightStatusEffect )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.seer_highlight_target, SonarBlast_StopHighlightStatusEffect )


	RegisterSignal( "SonicBlastReleased" )
	RegisterSignal( "SonicBlastCancelled" )
	RegisterSignal( "HitBySeerTact" )
	RegisterSignal( "PlayerHealthRevealed" )
}

void function OnWeaponActivate_ability_sonic_blast( entity weapon )
{
	entity weaponOwner = weapon.GetWeaponOwner()

	if ( IsValid( weaponOwner ) )
	{
		thread ChargeUpSound_Thread( weapon, weaponOwner )
	}
}

void function OnWeaponDeactivate_ability_sonic_blast( entity weapon )
{
	entity weaponOwner = weapon.GetWeaponOwner()




	if ( IsValid( weaponOwner ) )
	{
		weaponOwner.Signal( "EndHeartbeatSensorUI" )
	}
}

void function ChargeUpSound_Thread( entity weapon, entity weaponOwner )
{
	Assert( IsNewThread(), "Must be threaded off" )
	weapon.EndSignal( "OnPrimaryAttack" )
	weaponOwner.EndSignal( "OnDeath" )
	weaponOwner.EndSignal( "OnDestroy" )
	weaponOwner.EndSignal( "BleedOut_OnStartDying" )
	weaponOwner.EndSignal( "SonicBlastCancelled" )


	if ( weaponOwner != GetLocalViewPlayer() )
	{
		return
	}







	EmitSoundOnEntity( weaponOwner, SONIC_BLAST_INITIAL_CHARGE_1P )


	OnThreadEnd(
		function() : (  weaponOwner )
		{
			if ( IsValid( weaponOwner ) )
			{





				StopSoundOnEntity( weaponOwner, SONIC_BLAST_INITIAL_CHARGE_1P )

			}
		}
	)

	WaitForever()
}

bool function OnWeaponAttemptOffhandSwitch_weapon_sonic_blast( entity weapon )
{
	return true
}


var function OnWeaponTossCancel_weapon_sonic_blast( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity weaponOwner = weapon.GetWeaponOwner()
	weaponOwner.Signal( "EndHeartbeatSensorUI" )
	weaponOwner.Signal( "SonicBlastCancelled" )





	return 0
}

void function OnWeaponTossPrep_weapon_sonic_blast( entity weapon, WeaponTossPrepParams prepParams )
{
	weapon.PlayWeaponEffect( SONIC_BLAST_FX_HOLD_1P, SONIC_BLAST_FX_HOLD_3P, "R_HAND" )
	weapon.PlayWeaponEffect( SONIC_BLAST_FX_HOLD_1P, SONIC_BLAST_FX_HOLD_3P, "L_HAND" )
















	entity weaponOwner = weapon.GetWeaponOwner()
	if ( weaponOwner == GetLocalViewPlayer() )
	{
		thread DoHeartbeatSensorUI_Thread( weaponOwner, weapon )
	}

}

































































void function DoHeartbeatSensorUI_Thread( entity player, entity weapon )
{
	Assert ( IsNewThread(), "Must be threaded off." )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( "SonicBlastReleased" )
	player.EndSignal( "EndHeartbeatSensorUI" )
	player.EndSignal( "DestroyHeartbeatSensor" )
	weapon.EndSignal( "OnDestroy" )

	if ( file.heartbeatSensorActive )
		return

	file.heartbeatSensorActive = true

	OnThreadEnd(
		function() : ( player )
		{
			file.heartbeatSensorActive = false
			DeactivateHeartbeatSensor( player, true )
			player.Signal( "EndHeartbeatSensorUI" )
		}
	)

	

	wait HEARTBEAT_SENSOR_INITIAL_ACTIVATION_DELAY_DEFAULT

	InitializeHeartbeatSensorUI( player )
	ActivateHeartbeatSensor( player, true )

	while ( true )
	{
		if ( !weapon.IsWeaponActivated() )
			return

		WaitFrame()
	}
}


var function OnWeaponTossReleaseAnimEvent_weapon_sonic_blast( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity weaponOwner = weapon.GetWeaponOwner()
	Assert ( weaponOwner.IsPlayer() )
	weapon.Signal( "OnPrimaryAttack" )





	if ( weaponOwner == GetLocalViewPlayer() )
	{
		int weaponOwnerTeam = weaponOwner.GetTeam()
		vector startPos = weaponOwner.GetWorldSpaceCenter() + ( weaponOwner.GetViewVector() * SONIC_BLAST_IN_FRONT_START_DISTANCE )
		float blastDelay =  GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponClassName(), "sonic_blast_delay" )
		
		if ( InPrediction() && IsFirstTimePredicted() )
		{
			var secondChargeSoundHandle = EmitSoundAtPosition( weaponOwnerTeam, startPos, SONIC_BLAST_SECOND_CHARGE_1P )
		}
	}


	weaponOwner.Signal( "SonicBlastReleased" )

	weapon.StopWeaponEffect( SONIC_BLAST_FX_HOLD_1P, SONIC_BLAST_FX_HOLD_3P)
	weapon.StopWeaponEffect( SONIC_BLAST_FX_HOLD_1P, SONIC_BLAST_FX_HOLD_3P)

	if ( weaponOwner.IsPlayer() )
		PlayerUsedOffhand( weaponOwner, weapon )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}







































































































































































































































































































































































































































































































bool function SonicBlast_TargetEntityShouldBeHighlighted( entity ent )
{

		if( !IsValid(ent) )
			return false

		if( StatusEffect_HasSeverity( ent,  eStatusEffect.seer_highlight_target ) )
			return true


	return false
}


void function SonarBlast_StartHighlightStatusEffect( entity ent, int statusEffect, bool actuallyChanged )
{
	ManageHighlightEntity( ent )
}

void function SonarBlast_StopHighlightStatusEffect( entity ent, int statusEffect, bool actuallyChanged )
{
	ManageHighlightEntity( ent )
}


bool function ShouldBlastHitVictim( vector startPos, vector blastVector, vector blastVecNormalized, entity victim, int weaponOwnerTeam )
{
	if ( IsValid( victim ) && ( victim.IsPlayer() || victim.IsPlayerDecoy() || IsTrainingDummie( victim ) || IsCombatNPC( victim ) ) )
	{
		vector blastToPlayer = Normalize( victim.GetWorldSpaceCenter() - startPos )

		if( IsTrainingDummie( victim ) && !victim.IsEntAlive() ) 
		{
			return false
		}

		
		if ( DotProduct( blastVecNormalized, blastToPlayer ) > 0.0 )
		{
			if ( !IsFriendlyTeam( victim.GetTeam(), weaponOwnerTeam ) )
			{
				
				vector eyePosition = IsPlayerInCryptoDroneCameraView( victim ) ? victim.GetAttachmentOrigin( victim.LookupAttachment( "HEADFOCUS" ) ) : victim.EyePosition()

				vector projection = GetClosestPointOnLineSegment( startPos, blastVector, eyePosition )
				float distance = Distance( projection, eyePosition )

				bool passedDistanceCheck = ( distance <= file.sonicBlastRadius )

				if ( passedDistanceCheck )
				{
					return true
				}
			}
		}
	}

	return false
}


void function ServerCallback_ApplyScreenShake( entity player, vector victimToSonicOriginNormalized )
{
	if ( player == GetLocalViewPlayer() )
	{
		ClientScreenShake( 2.75, 5, 0.75, ZERO_VECTOR )
	}
}

void function ServerCallback_DoDamageIndicator( entity player, entity attacker )
{
	if ( IsValid( player ) && IsValid( attacker ) )
	{
		if ( player == GetLocalViewPlayer() )
		{
			vector damageOrigin = attacker.GetWorldSpaceCenter() + ( attacker.GetViewVector() * SONIC_BLAST_IN_FRONT_START_DISTANCE )
			DamageIndicators( damageOrigin, attacker, eDamageSourceId.mp_ability_sonic_blast )
		}
	}
}


void function ServerToClient_ShowHealthRUI( entity owner, entity victim, float duration )
{
	thread ServerToClient_ShowHealthRUI_Thread( owner, victim, duration )
}

void function ServerToClient_ShowHealthRUI_Thread( entity owner, entity victim, float duration )
{
	Assert ( IsNewThread(), "Must be threaded off." )

	if ( !IsValid( victim ) )
		return


		
		victim.Signal( "PlayerHealthRevealed" )
		victim.EndSignal( "PlayerHealthRevealed" )

	victim.EndSignal( "OnDestroy" )
	victim.EndSignal( "OnDeath" )




	float endTime = Time() + duration
	bool visible = true

#if DEV
	if ( SONIC_BLAST_DEBUG )
	{
		printt("ServerToClient_ShowHealthRUI_Thread - Showing HP Bars for " + victim.GetPlayerName())
	}
#endif

	OnThreadEnd(
		function() : ( owner, victim )
		{
			ReconScan_RemoveHudForTarget( owner, victim )
		}
	)

	var rui = ReconScan_ShowHudForTarget( owner, victim, false )

	while ( Time() < endTime )
	{
		bool phaseShifted = victim.IsPlayer() ? victim.IsPhaseShiftedOrPending() : false







			if ( phaseShifted )

		{
			if ( visible )
			{
				RuiSetBool( rui, "isVisible", false )
				visible = false
			}
		}
		else
		{
			if ( !visible )
			{
				RuiSetBool( rui, "isVisible", true )
				visible = true
			}
		}

		WaitFrame()
	}
}

void function ServerToClient_SpawnedSonicBlast( entity ownerPlayer, int ownerTeam, vector startPos, vector blastVector, float detonationTime )
{
	entity localViewPlayer = GetLocalViewPlayer()
	bool isEnemy           = IsEnemyTeam( ownerTeam, localViewPlayer.GetTeam() )

	var formSound = EmitSoundFromLine( startPos, blastVector, SONIC_BLAST_CYLINDER_FORM_3P )

	if ( isEnemy && ( localViewPlayer != ownerPlayer) )
	{
		thread DoSonicBlastThreatIndicator_Thread( localViewPlayer, startPos, blastVector, detonationTime )
	}

	thread DoClientSideDetonationSound_Thread( detonationTime, startPos, blastVector )
}

void function DoClientSideDetonationSound_Thread( float detonationTime, vector startPos, vector blastVector )
{
	Assert ( IsNewThread(), "Must be threaded off." )

	float deltaTime = 0.0

	if ( detonationTime > Time() )
	{
		deltaTime = detonationTime - Time()
	}

#if DEV
	if ( SONIC_BLAST_DEBUG )
	{
		printt(FUNC_NAME() + " deltaTime for blast: " + deltaTime )
	}
#endif

	wait deltaTime

#if DEV
	if ( SONIC_BLAST_DEBUG )
	{
		printt( FUNC_NAME() + " Sonic Blast Detonation at: " + Time() )
	}
#endif

	EmitSoundFromLine( startPos, blastVector, SONIC_BLAST_3P )
}

















































































































































void function DoSonicBlastThreatIndicator_Thread( entity victim, vector startPos, vector blastVector, float detonationTime )
{
	Assert ( IsNewThread(), "Must be threaded off." )
	victim.EndSignal( "OnDestroy" )

	vector projection = GetClosestPointOnLineSegment( startPos, blastVector, victim.EyePosition() )
	entity threatTarget = CreateClientSidePropDynamic( projection, <0,0,0>, $"mdl/dev/empty_model.rmdl" )

	OnThreadEnd(
		function() : (  threatTarget )
		{
			if ( IsValid( threatTarget ) )
			{
				threatTarget.Destroy()
			}
		}
	)

	threatTarget.SetScriptName( SONIC_BLAST_THREAT_TARGETNAME )
	ShowGrenadeArrow( victim, threatTarget, file.sonicBlastRadius, 0.0, false )

	while ( Time() < detonationTime )
	{
		projection = GetClosestPointOnLineSegment( startPos, blastVector, victim.EyePosition() )
		threatTarget.SetOrigin( projection )
		WaitFrame()
	}
}

float function GetSonicBlastSonarDuration()
{
	return GetCurrentPlaylistVarFloat( "seer_tac_sonar_duration", SONAR_DURATION )
}

float function GetSonicBlastSilenceDuration()
{
	return GetCurrentPlaylistVarFloat( "seer_tac_silence_duration", SILENCE_DURATION )
}

float function GetSonicBlastRadius()
{
	return GetCurrentPlaylistVarFloat( "seer_tac_radius", SONIC_BLAST_RADIUS )
}

bool function GetSonicBlastDoesDamage()
{
	return GetCurrentPlaylistVarBool( "seer_tac_does_damage", false )
}

int function GetSonicBlastDamage()
{
	return GetCurrentPlaylistVarInt( "seer_tac_damage", SONIC_BLAST_DAMAGE_AMOUNT )
}


float function GetSonicBlast_Duration_Extension()
{
	return GetCurrentPlaylistVarFloat( "sonicblast_scan_duration_upgraded_extension", 1.5 )
}

float function GetSonicBlast_Silence_Extension()
{
	return GetCurrentPlaylistVarFloat( "sonicblast_silence_duration_upgraded_extension", 2.0 )
}


float function GetSonicBlast_Scan_Duration_Base( entity player )
{
	float result = file.sonicBlastSonarDuration

	if( !IsValid( player ) )
		return result

	if( !player.IsPlayer() )
		return result


		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_ONE ) ) 
		{
			result += GetSonicBlast_Duration_Extension()
		}


	return result
}

float function GetSonicBlast_Silence_Duration_Base( entity player )
{
	float result = file.sonicBlastSilenceDuration

	if( !IsValid( player ) )
		return result

	if( !player.IsPlayer() )
		return result


		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
		{
			result += GetSonicBlast_Silence_Extension()
		}


	return result
}

bool function GetSonicBlastDoesSonarScan()
{
	return GetCurrentPlaylistVarBool( "seer_tac_does_sonar_scan", true )
}


bool function GetSonicBlastInterrupts()
{
	return GetCurrentPlaylistVarBool( "seer_tac_interrupts", true )
}


float function GetSonicBlastRange( entity player )
{

	
	if( PlayerHasPassive( player, ePassives.PAS_PAS_UPGRADE_ONE ) ) 
		return GetCurrentPlaylistVarFloat( "seer_passive_extended_range_upgraded", HEARTBEAT_SENSOR_NATURAL_RANGE_UPGRADE ) / INCHES_TO_METERS

	return GetHeartbeatSensorRange( player ) + SONIC_BLAST_RANGE_EXTENSION
}

