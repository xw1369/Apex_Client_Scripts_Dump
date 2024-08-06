global function MpAbilityMirageUltimate_Init
global function OnWeaponChargeBegin_ability_mirage_ultimate
global function OnWeaponChargeEnd_ability_mirage_ultimate
global function OnWeaponAttemptOffhandSwitch_ability_mirage_ultimate



const int CLOAK_TRIGGER_RADIUS = 240

const int MIRAGE_ULT_MAX_DECOYS = 5

global function GetMirageCloakDuration

struct
{

	var cancelHintRui

} file

void function MpAbilityMirageUltimate_Init()
{
	RegisterSignal( "CancelCloak" )
	PrecacheScriptString( CONTROLLED_DECOY_SCRIPTNAME )

	StatusEffect_RegisterEnabledCallback( eStatusEffect.mirage_ultimate_cancel_hint, CancelHint_OnCreate )
	StatusEffect_RegisterDisabledCallback( eStatusEffect.mirage_ultimate_cancel_hint, CancelHint_OnDestroy )

}


void function CancelHint_OnCreate( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() )
		return

	file.cancelHintRui = CreateFullscreenRui( $"ui/mirage_ultimate_cancel_hint.rpak" )
}

void function CancelHint_OnDestroy( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() )
		return

	RuiDestroyIfAlive( file.cancelHintRui )
	file.cancelHintRui = null
}


bool function OnWeaponAttemptOffhandSwitch_ability_mirage_ultimate( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return false

	if ( !PlayerCanUseDecoy( player ) )
		return false

	return true
}


var function OnWeaponPrimaryAttack_mirage_ultimate( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	var ammoToReturn = OnWeaponPrimaryAttack_holopilot( weapon, attackParams )




	return ammoToReturn
}

void function MirageUltimateCancelCloak( entity player )
{
	player.Signal( "CancelCloak")
}

void function OnWeaponChargeEnd_ability_mirage_ultimate( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return






	if ( weapon.GetWeaponChargeFraction() < 1 )
	{
		MirageUltimateCancelCloak( player )
		return
	}

	if ( weapon.GetWeaponPrimaryClipCount() == 0 ) 
		return


		int curAmmo = weapon.GetWeaponPrimaryClipCount()
		int maxAmmo = weapon.GetWeaponPrimaryClipCountMax()

		weapon.SetWeaponPrimaryClipCount( maxint( 0, curAmmo - weapon.GetAmmoPerShot() ) )



	if ( ShouldDoKaleidoscopeUltimate() )
	{



	}
	else
	{
		WeaponPrimaryAttackParams attackParams
		OnWeaponPrimaryAttack_mirage_ultimate( weapon, attackParams )
	}
}










































































float function GetMirageCloakDuration( entity player, float duration )
{

		if( PlayerHasPassive( player, ePassives.PAS_MIRAGE ) && PlayerHasPassive( player, ePassives.PAS_ULT_UPGRADE_ONE ) )
		{
			duration += GetMirageUpgradedCloakDuration()
		}

	return duration
}


float function GetMirageUpgradedCloakDuration()
{
	return GetCurrentPlaylistVarFloat( "upgrade_mirage_extra_cloak_duration", 1.0 )
}


bool function OnWeaponChargeBegin_ability_mirage_ultimate( entity weapon )
{
	weapon.EmitWeaponSound_1p3p( "Mirage_Vanish_Activate_1P", "Mirage_Vanish_Activate_3P" )
	entity ownerPlayer = weapon.GetWeaponOwner()
	PlayerUsedOffhand( ownerPlayer, weapon, true )























	return true
}


bool function ShouldDoKaleidoscopeUltimate()
{





	return GetCurrentPlaylistVarBool( "mirage_kaleidoscope_ulti_enabled", true )
}

int function MirageUltimate_GetMaxKaleidoscopeDecoys( entity player )
{
	int maxDecoys = MIRAGE_ULT_MAX_DECOYS

		if( player.HasPassive( ePassives.PAS_ULT_UPGRADE_TWO ) )
		{
			maxDecoys += 1
		}

	return maxDecoys
}






























































































































                                      