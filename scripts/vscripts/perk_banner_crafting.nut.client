
global function Perk_ExtraBinLoot_Init
global function Perk_ExpiredBannerRecovery_Enabled
global function Perk_Get_CraftedBannerTimeoutDuration



global function ServerCallback_Perk_ShowPlayerCraftingBannerGuidance



global function Perk_CanCraftBanners
global function Perk_CanBuyBanners
global function Perk_CanPickUpExpiredBanners
global function Perk_CanExpiredBannerBeRecovered
global function GetCraftableTeamBanners
global function Perk_CanTeammateCraftPlayerBanner
global function DoesTeammateHaveBannerCraftingPerk


global const string EXPIRED_BANNER_RECOVERY_NETVAR 		= "hasExpiredBannerPerk"
global const string DEATH_BOX_BANNER_EXPIRED_NETVAR 	= "DeathBoxBannerExpired"
global const string CRAFTED_BANNER_REF = "expired_banners"
global const string CRAFTED_BANNER_MODEL_NAME = "mdl/props/ultimate_accelerant/ultimate_accelerant_banner_crafting.rmdl"

const bool ALLOW_EXPIRED_BANNERS_ONLY = false

void function Perk_ExtraBinLoot_Init()
{
	if ( GetCurrentPlaylistVarBool( "disable_perk_extra_bin_loot", false ) )
		return

	PerkInfo bannerCrafting
	bannerCrafting.perkId = ePerkIndex.BANNER_CRAFTING

		bannerCrafting.activateCallback = OnActivate_ExpiredBannerRecoveryPerk
		bannerCrafting.deactivateCallback = OnDeactivate_ExpiredBannerRecoveryPerk
		bannerCrafting.minimapPingType = ePingType.SUPPORT_BOX
		Remote_RegisterClientFunction( "ServerCallback_Perk_ShowPlayerCraftingBannerGuidance" )


		AddCreateCallback( "prop_survival", OnCraftedBannerPropCreated )


	Perks_RegisterClassPerk( bannerCrafting )

	RegisterSignal( "RecoveredExpiredDNA" )
	RegisterSignal( "BannerCraftingDisabled" )







}

bool function Perk_ExpiredBannerRecovery_Enabled()
{
	return GetCurrentPlaylistVarBool( "perk_expired_banner_recovery_enabled", false )
}

bool function Perk_TeammatesCanCraftSupportBanners_Enabled()
{
	return GetCurrentPlaylistVarBool( "teammates_can_craft_support_banners_enabled", true )
}

bool function Perk_SupportCanCraftNonExpiredBanners_Enabled()
{
	return GetCurrentPlaylistVarBool( "support_can_craft_non_expired_banners", true )
}

float function Perk_Get_CraftedBannerTimeoutDuration()
{
	return GetCurrentPlaylistVarFloat( "perk_crafted_banner_timeout_duration", 90.0 )
}


void function OnActivate_ExpiredBannerRecoveryPerk( entity player, string characterName )
{



}














































































































void function OnDeactivate_ExpiredBannerRecoveryPerk( entity player )
{

























}




bool function CanCraftPlayersBanner( entity player )
{
	int respawnStatus = player.GetPlayerNetInt( "respawnStatus" )
	if( respawnStatus == eRespawnStatus.PICKUP_DESTROYED )
	{
		return true
	}
	else if( Perk_SupportCanCraftNonExpiredBanners_Enabled() && respawnStatus == eRespawnStatus.WAITING_FOR_PICKUP )
	{
		return true
	}
	return false
}

bool function Perk_CanTeammateCraftPlayerBanner( entity player )
{
	int gameState = GetGameState()
	if( gameState >= eGameState.WinnerDetermined || gameState < eGameState.Prematch )
		return false

	if( player.GetPlayerNetInt( "respawnStatus" ) == eRespawnStatus.SQUAD_ELIMINATED )
		return false

	foreach ( teamMember in GetPlayerArrayOfTeam( player.GetTeam() ) )
	{
		if( !Perk_TeammatesCanCraftSupportBanners_Enabled() )
		{
			if ( teamMember == player )
				continue

			if ( !IsAlive( teamMember ) && !PlayerIsMarkedAsCanBeRespawned( teamMember ) )
				continue
		}
		if( Perk_CanBuyBanners( teamMember ) )
		{
			return true
		}
	}

	return false
}

array<entity> function GetCraftableTeamBanners( entity player )
{
	array<entity> bannerPlayers

	if( !IsValid( player ) )
		return bannerPlayers

	foreach ( teamMember in GetPlayerArrayOfTeam( player.GetTeam() ) )
	{
		if ( teamMember == player )
			continue
		if( CanCraftPlayersBanner( teamMember ) )
		{
			bannerPlayers.append( teamMember )
		}
	}
	return bannerPlayers
}



















































bool function Perk_CanBuyBanners( entity player )
{
	array<entity> bannerPlayers = GetCraftableTeamBanners( player )
	if ( bannerPlayers.len() > 0 )
	{
		return Perk_CanCraftBanners( player )
	}

	return false
}


bool function Perk_CanCraftBanners( entity player )
{
	if( Perk_TeammatesCanCraftSupportBanners_Enabled() )
	{
		return player.GetPlayerNetBool( EXPIRED_BANNER_RECOVERY_NETVAR )
	}

	
	return Perks_DoesPlayerHavePerk( player, ePerkIndex.BANNER_CRAFTING )
}



bool function Perk_CanPickUpExpiredBanners( entity player, entity deathbox )
{
	if( !Perk_ExpiredBannerRecovery_Enabled() )
		return false

	entity deathboxOwner = deathbox.GetOwner()

	if( !IsValid( deathboxOwner ) )
		return false

	if( Perks_DoesPlayerHavePerk( player, ePerkIndex.BANNER_CRAFTING ) )
	{
		if( deathboxOwner == player )
			return false

		int ownerTeam = deathboxOwner.GetTeam()
		if ( !IsAlive(deathboxOwner ) && deathboxOwner.GetPlayerNetInt( "respawnStatus" ) == eRespawnStatus.WAITING_FOR_DELIVERY )
			return false

		if ( ownerTeam == player.GetTeam() && IsValid( deathbox.GetOwner() ) )
			return true
	}

	return false
}



bool function Perk_CanExpiredBannerBeRecovered( entity player ) 
{
	if( !Perk_ExpiredBannerRecovery_Enabled() )
		return false

	bool canRecoverBanner = false

	int team = player.GetTeam()
	array<entity> teamArray = GetPlayerArrayOfTeam( team )
	foreach ( entity ally in teamArray )
	{
		if( !IsValid( ally ) )
			continue
		
		
		

		
		if ( Perks_DoesPlayerHavePerk( ally, ePerkIndex.BANNER_CRAFTING ) ) 
			return true
	}

	return false
}



















































































































































void function ServerCallback_Perk_ShowPlayerCraftingBannerGuidance()
{

	if ( Crafting_IsDispenserCraftingEnabled() )
	{
		AddPlayerHint( 10.0, 1.0, $"", "#DISPENSERS_EXPIRED_BANNER_HINT" )
	}
	else


	AddPlayerHint( 10.0, 1.0, $"", "#CRAFT_EXPIRED_BANNER_HINT" )

	
	
}

void function Perk_HighlightReplicators( )
{
	MarkAllWorkbenches() 
	

	entity player = GetLocalViewPlayer()
	float duration = 15.0

	thread Thread_Perk_ClearDiageticReplicatorHighlights( player, duration )
}

void function Thread_Perk_ClearDiageticReplicatorHighlights( entity player, float duration )
{
	player.EndSignal( "OnDestroy" )

	wait duration

	OnThreadEnd(
		function() : ( )
		{
			DestroyWorkbenchMarkers()
		}
	)
}

void function OnCraftedBannerPropCreated( entity prop )
{
	if( prop.GetModelName() != CRAFTED_BANNER_MODEL_NAME )
		return
	LootData data = SURVIVAL_Loot_GetLootDataByIndex( prop.GetSurvivalInt() )
	if( data.ref != CRAFTED_BANNER_REF )
		return

	thread CreateCraftedBannerRui( prop )
}

const float MAGIC_DEATHBOX_Z_OFFSET = 1.25
void function CreateCraftedBannerRui( entity banner )
{
	EHI ornull ehi = banner.GetSurvivalProperty()
	if ( ehi == null )
		return

	expect EHI( ehi )

	banner.EndSignal( "OnDestroy" )

	float scale  = 0.025
	float width  = 264 * scale
	float height = 720 * scale
	vector right     = <0, 1, 0> * width * 0.5
	vector fwd       = <1, 0, 0> * height * 0.5

	vector org = <0, 0, .4>

	var topo = RuiTopology_CreatePlane( org - right * 0.5 - fwd * 0.5, right, fwd, true )
	RuiTopology_SetParent( topo, banner )

	NestedGladiatorCardHandle ornull nestedGCHandleOrNull = null

	var rui
	rui = RuiCreate( $"ui/gladiator_card_deathbox.rpak", topo, RUI_DRAW_WORLD, MINIMAP_Z_BASE + 10 )
	NestedGladiatorCardHandle nestedGCHandle = CreateNestedGladiatorCard( rui, "card", eGladCardDisplaySituation.DEATH_BOX_STILL, eGladCardPresentation.FRONT_DETAILS )
	nestedGCHandleOrNull = nestedGCHandle

	ChangeNestedGladiatorCardOwner( nestedGCHandle, ehi, null, eGladCardLifestateOverride.ALIVE )

	OnThreadEnd (
		void function() : ( topo, rui, nestedGCHandleOrNull )
		{
			if ( nestedGCHandleOrNull != null )
				CleanupNestedGladiatorCard( expect NestedGladiatorCardHandle( nestedGCHandleOrNull ) )
			RuiDestroy( rui )
			RuiTopology_Destroy( topo )
		}
	)

	WaitFrame()
	WaitForever()
}



bool function DoesTeammateHaveBannerCraftingPerk( entity player )
{
	bool hasPerk = false
	array<entity> teammates = GetPlayerArrayOfTeam( player.GetTeam() )
	foreach ( teamMember in teammates )
	{
		if ( Perks_DoesPlayerHavePerk( teamMember, ePerkIndex.BANNER_CRAFTING ) )
		{
			hasPerk = true
			break
		}
	}
	return hasPerk
}

                        