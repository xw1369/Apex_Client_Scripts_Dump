

global function Grenade_OnWeaponTossReleaseAnimEvent_RevShell
global function OnWeaponCustomActivityStart_weapon_Revshell
global function OnWeaponCustomActivityEnd_weapon_Revshell
global function OnWeaponActivate_RevShell
global function OnWeaponDeactivate_RevShell
global function RevShell_Init
global function IsRevShellEnabled


global function ClientRevShellTargetFX


const asset REV_SHELL_MODEL = $"mdl/weapons/skull_grenade/skull_grenade_base_v.rmdl"
const asset REV_SHELL_MODEL_V1 = $"mdl/weapons/skull_grenade/skull_grenade_base_v_v1.rmdl"
const asset VFX_REV_SHELL_DESTROY = $"P_destroy_exp_rev"
const asset VFX_REV_SHELL_FIZZLE = $"P_wpn_grenade_rev_fizzle"
const asset VFX_REV_SHELL_LOCK_ON = $"P_revNade_lockon_sprite"
const asset VFX_REV_SHELL_EYE_GLOW = $"P_wpn_grenade_rev_eyeglow"
const asset VFX_REV_SHELL_EYE_GLOW_VOICE = $"P_wpn_grenade_rev_eyeglow_voice"
global const string REV_SHELL_TARGETNAME = "Rev_shell"
global const string REV_SHELL_SCRIPTNAME = "revShell"

const string REV_SHELL_SHELLTARGETVICTIM_SOUND = "diag_mp_nocNotify_bc_shellTargetVictim_3p"
const string REV_SHELL_SHELLTARGE_SOUND = "diag_mp_nocNotify_bc_shellSeek_3p"
const string REV_SHELL_SHELLTAUNTHUMAN = "diag_mp_nocNotify_bc_shellTauntHuman_1p"
const string REV_SHELL_SHELLTAUNTROBOT = "diag_mp_nocNotify_bc_shellTauntRobot_1p"
const string REV_SHELL_SHELLTAUNTREV = "diag_mp_nocNotify_bc_shellTauntRevenant_1p"
const string REV_SHELL_DESTROY_SOUND = "explo_revshellseeker_destroyed_3p"
const string REV_SHELL_LOCKED_ON_VICTIM_SOUND = "ui_revshellseeker_victim_targetlock_1p"
const string REV_SHELL_LOCKED_ON_AGGRESSOR_SOUND = "ui_revshellseeker_aggressor_targetlock_1p"
const string REV_SHELL_ON_THROWN = "weapon_revshellseeker_seekingloop_3p"
const string REV_SHELL_SHELL_TOSS = "diag_mp_nocNotify_bc_shellToss_3p"
const string REV_SHELL_WARNING_BEEP = "weapon_revshellseeker_explosivewarningbeep_3p"


const float SPEED_CHANGE_DISTANCE_THRESHOLD = 800.0 
global const float REV_SHELL_GIVE_UP_TIME = 30 
float CLOSE_CHASE_SPEED = 240.0 
float FAR_CHASE_SPEED = 660.0 

const vector HEIGHT_OFFSET = < 0.0, 0.0, 50.0 >

string shellTauntVoice = REV_SHELL_SHELLTAUNTHUMAN

struct PathingState
{
	array< vector >	waypoints
	bool 			finished = false
	entity			target = null
	float			velocity = 660.0
}

void function RevShell_Init()
{
	RegisterSignal( "RevShellTauntEnd" )
	RegisterSignal( "RevShellEnd" )
	PrecacheModel( REV_SHELL_MODEL )
	PrecacheModel( REV_SHELL_MODEL_V1 )
	PrecacheParticleSystem( VFX_REV_SHELL_DESTROY )
	PrecacheParticleSystem( VFX_REV_SHELL_FIZZLE )
	PrecacheParticleSystem( VFX_REV_SHELL_LOCK_ON )
	PrecacheParticleSystem( VFX_REV_SHELL_EYE_GLOW )
	PrecacheParticleSystem( VFX_REV_SHELL_EYE_GLOW_VOICE )
	Remote_RegisterClientFunction( "ClientRevShellTargetFX", "entity", "entity" )

	SURVIVAL_Loot_RegisterConditionalCheck( "mp_weapon_grenade_rev_shell", Rev_Shell_ConditionalCheck )

	CLOSE_CHASE_SPEED = GetCurrentPlaylistVarFloat( "rev_shell_close_chase_speed", CLOSE_CHASE_SPEED )
	FAR_CHASE_SPEED = GetCurrentPlaylistVarFloat( "rev_shell_far_chase_speed", FAR_CHASE_SPEED )

}

void function OnWeaponActivate_RevShell( entity weapon )
{
	Grenade_OnWeaponActivate( weapon )


	thread RevShell_PlayTauntVoice_Thread( weapon )
	thread RevShell_HandleEyeGlowVFX_Voice_Thread( weapon )

}

void function OnWeaponDeactivate_RevShell(entity weapon)
{
	Grenade_OnWeaponDeactivate( weapon )
	Signal( weapon, "RevShellTauntEnd" )
}

void function OnWeaponCustomActivityStart_weapon_Revshell( entity weapon, int sequence )
{

		
		if ( weapon.GetWeaponActivity() == ACT_VM_WEAPON_INSPECT )
		{
			StopSoundOnEntityByName( weapon, shellTauntVoice )
		}

}

void function OnWeaponCustomActivityEnd_weapon_Revshell( entity weapon, int sequence )
{
	
}


void function RevShell_PlayTauntVoice_Thread( entity weapon )
{
	entity weaponOwner = weapon.GetWeaponOwner()

	if ( IsValid( weaponOwner ) == false )
		return

	if ( weaponOwner != GetLocalViewPlayer() )
		return

	EndSignal( weapon, "RevShellTauntEnd" )
	EndSignal( weapon, "OnDestroy" )

	string playerClass = ItemFlavor_GetCharacterRef( LoadoutSlot_GetItemFlavor( ToEHI( weaponOwner ), Loadout_Character() ) )

	if ( playerClass == "character_revenant" )
	{
		shellTauntVoice = REV_SHELL_SHELLTAUNTREV
	}
	else if ( playerClass == "character_ash" || playerClass == "character_pathfinder" )
	{
		shellTauntVoice = REV_SHELL_SHELLTAUNTROBOT
	}

	while ( true )
	{
		wait RandomFloatRange( 3.0, 5.0 )
		if ( weapon.GetWeaponActivity() != ACT_VM_WEAPON_INSPECT )
		{
			EmitSoundOnEntity( weapon, shellTauntVoice )
		}
		wait RandomFloatRange( 7.0, 10.0 )
	}
}


var function Grenade_OnWeaponTossReleaseAnimEvent_RevShell( entity weapon, WeaponPrimaryAttackParams attackParams )
{

		if ( InPrediction() && !IsFirstTimePredicted() )
			return false

		if ( !weapon.ShouldPredictProjectiles() )
			return false

		StopSoundOnEntityByName( weapon, shellTauntVoice )



	entity weaponOwner = weapon.GetWeaponOwner()

	weaponOwner.Signal( "ThrowGrenade" )

	int damageFlags = weapon.GetWeaponDamageFlags()

	WeaponFireGrenadeParams fireGrenadeParams
	fireGrenadeParams.pos = attackParams.pos
	fireGrenadeParams.vel = Normalize( attackParams.dir )
	fireGrenadeParams.angVel = <0, 0, 0>
	fireGrenadeParams.fuseTime = 0
	fireGrenadeParams.scriptTouchDamageType = (damageFlags & ~DF_EXPLOSION) 
	fireGrenadeParams.scriptExplosionDamageType = damageFlags
	fireGrenadeParams.clientPredicted = PROJECTILE_PREDICTED
	fireGrenadeParams.lagCompensated = PROJECTILE_NOT_LAG_COMPENSATED
	fireGrenadeParams.useScriptOnDamage = true

	int team = weaponOwner.GetTeam()

	entity grenade = weapon.FireWeaponGrenade( fireGrenadeParams )
	SetTeam( grenade, team )
	grenade.SetScriptName( REV_SHELL_SCRIPTNAME )
	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )
	EmitSoundOnEntity( grenade, REV_SHELL_SHELL_TOSS )
	PlayerUsedOffhand( weaponOwner, weapon, true, grenade ) 





























	return weapon.GetAmmoPerShot()
}































void function OnRevShellPostDamaged( entity grenadeProxy, var damageInfo )
{













}






















































































































































































































































































































































































































































































void function ClientRevShellTargetFX( entity target, entity projectile )
{
	thread ClientRevShellTargetFX_Thread( target, projectile )
}

void function ClientRevShellTargetFX_Thread( entity target, entity projectile )
{
	if ( !IsValid( target ) || !IsValid( projectile ) )
		return

	EndSignal( projectile, "RevShellEnd" )
	EndSignal( projectile, "OnDestroy" )

	int fxIndex = GetParticleSystemIndex( VFX_REV_SHELL_LOCK_ON )
	int attachIdx = target.LookupAttachment( "CHESTFOCUS" )
	Assert( attachIdx != 0, "Attachment index is 0 for some reason, consider checking IsPlayer()" )
	if ( attachIdx <= 0 )
	{
		return
	}

	int effectHandle = StartParticleEffectOnEntity( target, fxIndex, FX_PATTACH_POINT_FOLLOW, attachIdx )

	OnThreadEnd(
		function() : ( effectHandle )
		{
			if ( EffectDoesExist( effectHandle ) )
			{
				EffectStop( effectHandle, true, true )
			}
		}
	)

	WaitForever()
}

void function RevShell_HandleEyeGlowVFX_Voice_Thread( entity weapon )
{
	if ( !IsValid( weapon ) )
		return

	entity viewmodel = weapon.GetWeaponViewmodel()

	if ( !IsValid( viewmodel ) )
		return

	EndSignal( weapon, "RevShellTauntEnd" )
	EndSignal( weapon, "RevShellEnd" )
	EndSignal( weapon, "OnDestroy" )

	int fxIndex = GetParticleSystemIndex( VFX_REV_SHELL_EYE_GLOW_VOICE )
	int attachIdxL = viewmodel.LookupAttachment( "L_EYE_VFX" )
	int attachIdxR = viewmodel.LookupAttachment( "R_EYE_VFX" )
	Assert( attachIdxL != 0, "Attachment index is 0 for some reason, check rev shell" )
	Assert( attachIdxR != 0, "Attachment index is 0 for some reason, check rev shell" )
	if ( attachIdxL <= 0 || attachIdxR <= 0 )
	{
		return
	}

	int effectHandleL = StartParticleEffectOnEntity( viewmodel, fxIndex, FX_PATTACH_POINT_FOLLOW, attachIdxL )
	int effectHandleR = StartParticleEffectOnEntity( viewmodel, fxIndex, FX_PATTACH_POINT_FOLLOW, attachIdxR )

	OnThreadEnd(
		function() : ( effectHandleL, effectHandleR )
		{
			if ( EffectDoesExist( effectHandleL ) )
				EffectStop( effectHandleL, false, true )

			if ( EffectDoesExist( effectHandleR ) )
				EffectStop( effectHandleR, false, true )
		}
	)

	WaitForever()
}

























































































bool function IsRevShellEnabled()
{
	return GetCurrentPlaylistVarBool( "rev_shell_enabled", false )
}

bool function Rev_Shell_ConditionalCheck( string ref, entity player )
{
	if ( !IsValid( player ) )
		return false

	int itemCount = SURVIVAL_CountItemsInInventory( player, ref )

	
	if ( itemCount > 0 )
		return true

	return IsRevShellEnabled()
}


