global function MpAbilityPhaseWalk_Init

global function OnWeaponActivate_ability_phase_walk
global function OnWeaponAttemptOffhandSwitch_ability_phase_walk
global function OnWeaponChargeBegin_ability_phase_walk
global function OnWeaponChargeEnd_ability_phase_walk

const string SOUND_ACTIVATE_1P = "pilot_phaseshift_firstarmraise_1p" 
const string SOUND_ACTIVATE_3P = "pilot_phaseshift_firstarmraise_3p" 

const float PHASE_WALK_PRE_TELL_TIME = 1.5
const asset PHASE_WALK_APPEAR_PRE_FX = $"P_phase_dash_pre_end_mdl"

const float SPEEDBOOST_STRENGTH = 0.3




struct
{



} file

void function MpAbilityPhaseWalk_Init()
{
	PrecacheParticleSystem( PHASE_WALK_APPEAR_PRE_FX )
}

bool function MpAbilityPhaseWalk_ShouldUseQuickPhase( entity weapon )
{
	return weapon.HasMod( "ult_active" ) || weapon.HasMod( "gamemode_dayzero" )
}

void function OnWeaponActivate_ability_phase_walk( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	float deploy_time = weapon.GetWeaponSettingFloat( eWeaponVar.deploy_time )








	if ( !MpAbilityPhaseWalk_ShouldUseQuickPhase( weapon ) )
	{





			if ( !InPrediction() )
				return

			EmitSoundOnEntity( player, SOUND_ACTIVATE_1P )


		float amount = GetCurrentPlaylistVarFloat( "wraith_phase_walk_slow_amount", 0.2 )
		StatusEffect_AddTimed( player, eStatusEffect.move_slow, amount, deploy_time, deploy_time )
	}
}

bool function OnWeaponAttemptOffhandSwitch_ability_phase_walk( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( IsValid( player ) && player.IsPhaseShifted() )
		return false

	return true
}













































bool function OnWeaponChargeBegin_ability_phase_walk( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	float chargeTime = weapon.GetWeaponSettingFloat( eWeaponVar.charge_time )

	
	{
		bool doStatus = true

			if ( !InPrediction() )
				doStatus = false


		if ( doStatus )
		{














			{
				int speedHandle = StatusEffect_AddTimed( player, eStatusEffect.speed_boost, SPEEDBOOST_STRENGTH, chargeTime, chargeTime * 0.3 )




			}
		}
	}














	PhaseShift( player, 0, chargeTime, PHASETYPE_BALANCE )

	player.Signal( "WreckingBall_CleanupFX" )

	return true
}




































































void function OnWeaponChargeEnd_ability_phase_walk( entity weapon )
{
	entity player = weapon.GetWeaponOwner()














}