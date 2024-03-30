global function MpWeaponBubbleBunker_Init

global function OnWeaponTossReleaseAnimEvent_WeaponBubbleBunker
global function OnWeaponTossPrep_WeaponBubbleBunker









global function GibraltarIsInDome
global function InDomeShield

global const string GIBRALTAR_DOME_SCRIPTNAME = "gibraltar_dome_shield"
global const string BUBBLE_BUNKER_MOVER_SCRIPTNAME = "Gibraltar_BubbleShield_mover"
global const string BUBBLE_BUNKER_WEAPON_NAME = "mp_weapon_bubble_bunker"

const float BUBBLE_BUNKER_DEPLOY_DELAY = 1.0
const float BUBBLE_BUNKER_DURATION_WARNING = 5.0

const bool BUBBLE_BUNKER_DAMAGE_ENEMIES = false

const float BUBBLE_BUNKER_ANGLE_LIMIT = 0.55






const asset BUBBLE_BUNKER_BEAM_FX = $"P_wpn_BBunker_beam"
const asset BUBBLE_BUNKER_BEAM_END_FX = $"P_wpn_BBunker_beam_end"
const asset BUBBLE_BUNKER_SHIELD_FX = $"P_wpn_BBunker_shield"
const asset BUBBLE_BUNKER_SHIELD_COLLISION_MODEL = $"mdl/fx/bb_shield.rmdl"
const asset BUBBLE_BUNKER_SHIELD_PROJECTILE = $"mdl/props/gibraltar_bubbleshield/gibraltar_bubbleshield.rmdl"







const asset BUBBLE_BUNKER_SHIELD_COLLISION_MODEL_SMALL = $"mdl/fx/bb_shield_small.rmdl"
const asset BUBBLESHIELD_FX_ASSET_SMALL = $"P_wpn_BBunker_shield_small"
const asset BUBBLE_BUNKER_SMALL_BEAM_FX = $"P_wpn_BBunker_beam_small"
const asset BUBBLE_BUNKER_SMALL_BEAM_END_FX = $"P_wpn_BBunker_beam_small_end"
const string BUBBLE_BUNKER_SOUND_ENDING_UPGRADE = "Gibraltar_BabyBubbleShield_LegendUpgrade_Ending"
const string BUBBLE_BUNKER_SOUND_FINISH_UPGRADE = "Gibraltar_BabyBubbleShield_LegendUpgrade_Deactivate"


const string BUBBLE_BUNKER_SOUND_ENDING = "Gibraltar_BubbleShield_Ending"
const string BUBBLE_BUNKER_SOUND_FINISH = "Gibraltar_BubbleShield_Deactivate"

const BUBBLE_BUNKER_THROW_POWER = 800.0
const BUBBLE_BUNKER_RADIUS = 240 

























void function MpWeaponBubbleBunker_Init()
{
	PrecacheParticleSystem( BUBBLE_BUNKER_BEAM_END_FX )
	PrecacheParticleSystem( BUBBLE_BUNKER_BEAM_FX )
	PrecacheParticleSystem( BUBBLE_BUNKER_SHIELD_FX )
	PrecacheModel( BUBBLE_BUNKER_SHIELD_COLLISION_MODEL )
	PrecacheModel( BUBBLE_BUNKER_SHIELD_PROJECTILE )

	PrecacheScriptString( GIBRALTAR_DOME_SCRIPTNAME )
	PrecacheScriptString( BUBBLE_BUNKER_MOVER_SCRIPTNAME )







		PrecacheParticleSystem( BUBBLESHIELD_FX_ASSET_SMALL )
		PrecacheParticleSystem( BUBBLE_BUNKER_SMALL_BEAM_FX )
		PrecacheParticleSystem( BUBBLE_BUNKER_SMALL_BEAM_END_FX )
		PrecacheModel( BUBBLE_BUNKER_SHIELD_COLLISION_MODEL_SMALL )		










	
	






}



























float function BubbleBunker_BaseScaler()
{
	return GetCurrentPlaylistVarFloat( "passive_upgrade_gibraltar_bunker_throw_base_scaler", 1.0 )
}

float function BubbleBunker_UpgradedScaler()
{
	return GetCurrentPlaylistVarFloat( "passive_upgrade_gibraltar_bunker_throw_upgraded_scaler", 1.1 )
}


float function BubbleBunker_GetThrowPower( entity player )
{
	float result = BUBBLE_BUNKER_THROW_POWER


	if( UpgradeCore_IsEnabled() )
	{
		result *= BubbleBunker_BaseScaler()
		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_THREE ) ) 
		{
			result *= BubbleBunker_UpgradedScaler()
		}
	}


	return result
}

var function OnWeaponTossReleaseAnimEvent_WeaponBubbleBunker( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	int ammoReq = weapon.GetAmmoPerShot()
	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )

	entity deployable = ThrowDeployable( weapon, attackParams, BubbleBunker_GetThrowPower( weapon.GetOwner() ), OnBubbleBunkerPlanted, null, null )
	if ( deployable )
	{
		entity player = weapon.GetWeaponOwner()
		PlayerUsedOffhand( player, weapon, true, deployable )














	}

	return ammoReq
}

void function OnWeaponTossPrep_WeaponBubbleBunker( entity weapon, WeaponTossPrepParams prepParams )
{
	weapon.EmitWeaponSound_1p3p( GetGrenadeDeploySound_1p( weapon ), GetGrenadeDeploySound_3p( weapon ) )
}

void function OnBubbleBunkerPlanted( entity projectile, DeployableCollisionParams collisionParams )
{







































































}


































































































































































































































































































































































































































































































































































































bool function GibraltarIsInDome( entity player )
{
	if ( !PlayerHasPassive( player, ePassives.PAS_ADS_SHIELD ) )
		return false

	return InDomeShield( player )
}

bool function InDomeShield( entity player )
{
	return StatusEffect_HasSeverity( player, eStatusEffect.bubble_bunker )
}