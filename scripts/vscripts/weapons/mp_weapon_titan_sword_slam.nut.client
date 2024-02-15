
global function MpWeaponTitanSword_Slam_Init
global function TitanSword_Slam_OnWeaponActivate
global function TitanSword_Slam_OnWeaponDectivate
global function TitanSword_Slam_ClearMods
global function TitanSword_Slam_TrySlam
global function TitanSword_Slam_VictimHitOverride
global function TitanSword_Slam_TrySlamAnimEvent








const string TITAN_SWORD_SLAM_READY_MOD = "slam_ready"
const string TITAN_SWORD_SLAM_MOD = "slam"

const string TITAN_SWORD_LOOT_MOVER_SCRIPTNAME = "titan_sword_loot_mover"




global const string SIG_TITAN_SWORD_SLAM_ACTIVATED = "TitanSword_SlamActivated"
global const string SIG_TITAN_SWORD_SLAM_LANDED = "TitanSword_SlamLanded"



const bool TITAN_SWORD_LOS_DEBUG = false


const int TITAN_SWORD_SLAM_DISABLED_WEAPON_TYPES = WPT_ULTIMATE | WPT_TACTICAL | WPT_CONSUMABLE


const asset VFX_TITAN_SWORD_SLAM = $"P_pilot_swd_slam_shockwave" 
const string VFX_TITAN_SWORD_SLAM_IMPACT = "pilot_sword_slam"
const TITAN_SWORD_FX_SLAM_ATK_FP = $"P_pilot_sword_swipe_slam_FP"
const TITAN_SWORD_FX_SLAM_ATK_3P = $"P_pilot_sword_swipe_slam_3P"
const asset VFX_TITAN_SWORD_SLAM_JETS = $"P_pilot_slam_thrusters"



const string SFX_TITAN_SWORD_SLAM_ACTIVATED_1P = "titansword_special_slam_activate_1p"
const string SFX_TITAN_SWORD_SLAM_AIR_1P = "titansword_special_slam_air_1p"
const string SFX_TITAN_SWORD_SLAM_AIR_3P = "titansword_special_slam_air_3p"


struct
{





		bool slamHintActive = false

}file

void function MpWeaponTitanSword_Slam_Init()
{
	RegisterImpactTable( VFX_TITAN_SWORD_SLAM_IMPACT )

	PrecacheParticleSystem( VFX_TITAN_SWORD_SLAM )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SLAM_JETS )


	PrecacheParticleSystem( TITAN_SWORD_FX_SLAM_ATK_FP )
	PrecacheParticleSystem( TITAN_SWORD_FX_SLAM_ATK_3P )

	RegisterSignal( SIG_TITAN_SWORD_SLAM_ACTIVATED )

	RegisterSignal( SIG_TITAN_SWORD_SLAM_LANDED )
}

void function TitanSword_Slam_StartVFX( entity weapon )
{
	
	weapon.PlayWeaponEffect( TITAN_SWORD_FX_SLAM_ATK_FP, TITAN_SWORD_FX_SLAM_ATK_3P, "muzzle_flash" )
}

void function TitanSword_Slam_OnWeaponActivate( entity player, entity weapon )
{
	
	








		thread TitanSword_SlamHint_Thread( player, weapon )

}

void function TitanSword_Slam_OnWeaponDectivate( entity player, entity weapon )
{
	
	

	ClearSlamState( player, weapon, false )
}

void function TitanSword_Slam_ClearMods( entity weapon )
{
	
	
	if ( weapon.HasMod( TITAN_SWORD_SLAM_MOD ) )
	{
		weapon.RemoveMod( TITAN_SWORD_SLAM_MOD )
	}
}









bool function TitanSword_Slam_TrySlam( entity player, entity weapon )
{

		if ( !InPrediction() )
			return false


	if ( player.IsOnGround() )
		return false

	if ( !weapon.HasMod( TITAN_SWORD_SLAM_READY_MOD ) )
		return false

	if ( weapon.HasMod( TITAN_SWORD_SLAM_MOD ) )
		return true

	weapon.RemoveMod( TITAN_SWORD_SLAM_READY_MOD )

	TitanSword_Slam_StartVFX( weapon )

	thread Slam_Thread( player, weapon )

	return true
}

void function TitanSword_Slam_TrySlamAnimEvent( entity weapon )
{

		if ( !InPrediction() )
			return


	if ( !weapon.HasMod( TITAN_SWORD_SLAM_MOD ) )
		return

	printt( "SLAMMING" )

	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) )
		return

	float velZ = GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "slam_vel_z" )
	player.PlayerLaunch( <0, 0, velZ>, false )
}






































































void function TitanSword_SlamHint_Thread( entity player, entity weapon )
{
	if ( !IsValid( player ) )
		return

	if ( !IsLocalViewPlayer( player ) )
		return

	if ( file.slamHintActive )
		return

	if ( !IsValid( weapon ) )
		return

	EndSignal( player, "BleedOut_OnStartDying" )
	EndSignal( player, "OnDeath" )
	EndSignal( player, "OnDestroy" )

	EndSignal( weapon, "OnDestroy" )
	EndSignal( weapon, SIG_TITAN_SWORD_DEACTIVATE )

	file.slamHintActive = true

	string[1] displayedHint = [""]

	float buildUp = -1

	OnThreadEnd(
		function() : (displayedHint)
		{
			if ( displayedHint[0] != "" )
				HidePlayerHint( displayedHint[0] )
			file.slamHintActive = false
		}
	)

	while( true )
	{
		string hint

		if ( CanSlam( player ) && weapon.HasMod( TITAN_SWORD_SLAM_READY_MOD ) && !weapon.HasMod( TITAN_SWORD_SLAM_MOD ) && !TitanSword_Block_IsBlocking( weapon ) )
		{
			if ( buildUp == -1 )
			{
				buildUp = Time() + 0.25
			}
			else if ( Time() > buildUp )
			{
				hint = "#WPN_TITAN_SWORD_SLAM_HINT_ALT"
			}
		}

		if ( hint != displayedHint[0] )
		{
			if ( displayedHint[0] != "" )
			{
				buildUp = -1
				HidePlayerHint( displayedHint[0] )
			}
			if ( hint != "" )
			{
				AddPlayerHint( 60.0, 0.0, $"", hint )
			}
			displayedHint[0] = hint
		}
		WaitFrame()
	}
}



bool function CanSlam( entity player )
{
	if ( player.IsOnGround() )
		return false

	if ( player.IsZiplining() )
		return false

	if ( StatusEffect_HasSeverity( player, eStatusEffect.in_space_elevator ) )
		return false

	return true
}

void function Slam_Thread( entity player, entity weapon )
{
	player.Signal( SIG_TITAN_SWORD_SLAM_ACTIVATED )

	player.EndSignal( SIG_TITAN_SWORD_SLAM_ACTIVATED )
	player.EndSignal( "JumpPadStart" )
	player.EndSignal( "JumpPadStart" )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )
	
	

	weapon.EndSignal( "OnDestroy" )
	
	weapon.EndSignal( SIG_TITAN_SWORD_DEACTIVATE )

	TitanSword_SafelyAddAttackMod( weapon, TITAN_SWORD_SLAM_MOD )

	player.PlayerLaunch( <0, 0, 200>, false )

	
	player.Signal( "JumpPad_GiveDoubleJump" )

		if ( TitanSword_ClientPredictCheck( "slam" ) )
		{
			EmitSoundOnEntity( player, SFX_TITAN_SWORD_SLAM_ACTIVATED_1P )
		}











































	wait 0.4 










	printt( "SHOULD SLAM NOW" )
	player.EndSignal( SIG_TITAN_SWORD_SLAM_LANDED )

	while( !player.IsOnGround() )
	{
		WaitFrame()
	}

	printt( "SLAM TOUCH GROUND" )
	OnPlayerSlamLand( player )
}

void function ClearSlamState( entity player, entity weapon, bool doLandAnim )
{
	if ( !IsValid( player ) )
		return

	if ( player.PlayerMelee_GetState() != PLAYER_MELEE_STATE_SLAM_ATTACK )
		return

	player.PlayerMelee_EndAttack()









		if ( !InPrediction() )
			return


	if ( !IsValid( weapon ) )
		return

	if ( weapon.IsInCustomActivity() )
		weapon.StopCustomActivity()

	if ( doLandAnim )
		weapon.StartCustomActivity( "ACT_VM_LAND_CUSTOM", WCAF_NONE )

	if ( IsValid( weapon ) )
		weapon.RemoveMod( TITAN_SWORD_SLAM_MOD )
}

void function OnPlayerSlamLand( entity player )
{
	printt( "SLAM LANDED!" )

	if ( player.PlayerMelee_GetState() != PLAYER_MELEE_STATE_SLAM_ATTACK )
		return

	entity weapon = TitanSword_GetMainWeapon( player )

	if ( !IsValid( weapon ) ) 
	{
		player.PlayerMelee_EndAttack()
		player.Signal( SIG_TITAN_SWORD_SLAM_LANDED ) 
		PlayRotatedImpactFXTable( player, player.GetOrigin(), AnglesToForward( player.GetAngles() ), VFX_TITAN_SWORD_SLAM_IMPACT )
		return
	}

	ClearSlamState( player, weapon, true )







	vector origin = player.GetOrigin()
	vector fwd    = AnglesToForward( player.GetAngles() )
	const float fwdImpactOffset = 50
	origin += Normalize( fwd ) * fwdImpactOffset
	PlayRotatedImpactFXTable( player, origin, AnglesToForward( player.GetAngles() ), VFX_TITAN_SWORD_SLAM_IMPACT, 0 )


		StartParticleEffectInWorld( GetParticleSystemIndex( VFX_TITAN_SWORD_SLAM ), player.GetOrigin(), <0, 0, 0> )


	player.Signal( SIG_TITAN_SWORD_SLAM_LANDED ) 
}


















































































































































































bool function TitanSword_Slam_VictimHitOverride( entity weapon, entity attacker, entity victim, vector velocity )
{
	if ( weapon.HasMod( TITAN_SWORD_SLAM_MOD ) )
	{
		if ( !IsFriendlyTeam( attacker.GetTeam(), victim.GetTeam() ) )
		{
			vector lookDirection = attacker.GetViewForward()
			lookDirection.z = 0
			lookDirection   = Normalize( lookDirection )

			vector knockback = lookDirection * 2000
			TitanSword_LaunchEntity( victim, <knockback.x, knockback.y, -2200> )
			return true
		}
	}

	return false
}




























                               