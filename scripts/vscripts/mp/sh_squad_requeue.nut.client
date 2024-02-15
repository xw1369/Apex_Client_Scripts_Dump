global function ClSquadRequeue_RegisterNetworkFunctions
global function SvSquadRequeue_RegisterNetworkFunctions



global function ServerCallback_SquadRequeueOptIn
global function ServerCallback_SquadRequeueOptOut
global function ServerCallback_SquadRequeuePartyMerge
global function ClSquadRequeue_OptIn











struct
{
	array<EHI> squadOptIns
	string optInLeaderUid
	bool amIOptedIn
	bool partyUpdateCallbackAdded
	float optInStartTime = -1.0
} clFile




























void function ClSquadRequeue_RegisterNetworkFunctions()
{
	Remote_RegisterClientFunction( "ServerCallback_SquadRequeueOptIn", "entity", "float", 0.0, FLT_MAX, 32 )
	Remote_RegisterClientFunction( "ServerCallback_SquadRequeueOptOut", "int", 0, INT_MAX )
	Remote_RegisterClientFunction( "ServerCallback_SquadRequeuePartyMerge", "int", 0, INT_MAX, "bool" )
}

void function SvSquadRequeue_RegisterNetworkFunctions()
{
	Remote_RegisterServerFunction( "ClientCallback_SquadRequeueOptIn", "typed_entity", "player" )
	Remote_RegisterServerFunction( "ClientCallback_SquadRequeueOptOut" )
}



entity ornull function ClGetLeaderIfPartyMembersInSquad()
{
	entity player = GetLocalClientPlayer()
	table< string, bool > activeSquadMembers
	Party party = GetParty()
	int team = player.GetTeam()
	entity leaderPlayer = null

	foreach ( entity teammate in GetPlayerArrayOfTeam( team ) )
	{
		if ( IsValid( teammate ) && teammate.IsConnectionActive() )
		{
			string uid = teammate.GetUserID()
			activeSquadMembers[uid] <- true
			if ( uid == party.originatorUID )
				leaderPlayer = teammate
		}
	}

	foreach ( partyMember in party.members )
	{
		if ( !( partyMember.uid in activeSquadMembers ) )
			return null
	}

	return leaderPlayer
}

void function OnPartyUpdated_AfterOptIn()
{
	if ( !clFile.amIOptedIn )
		return

	entity player = GetLocalClientPlayer()
	string uid = player.GetUserID()

	if ( clFile.optInLeaderUid == uid )
	{
		
		entity ornull leaderPlayer = ClGetLeaderIfPartyMembersInSquad()
		if ( leaderPlayer == null )
			ClSquadRequeue_OptOut()
	}
	else
	{
		
		
		Party party = GetParty()
		if ( party.originatorUID != clFile.optInLeaderUid )
		{
			if ( party.originatorUID == uid || ClGetLeaderIfPartyMembersInSquad() == null )
				ClSquadRequeue_OptOut()
		}
	}
}

void function ServerCallback_SquadRequeueOptOut( EHI optOutPlayerEHI )
{
	if ( optOutPlayerEHI == EHI_null )
		return

	int idx = clFile.squadOptIns.find( optOutPlayerEHI )
	if ( idx == -1 )
		return

	if ( LocalClientEHI() == optOutPlayerEHI )
	{
		clFile.amIOptedIn = false
		if ( clFile.partyUpdateCallbackAdded )
		{
			clFile.partyUpdateCallbackAdded = false
			RemoveCallback_OnPartyUpdated( OnPartyUpdated_AfterOptIn )
		}
	}

	clFile.squadOptIns.remove( idx )

	if ( clFile.squadOptIns.len() == 0 )
		clFile.optInStartTime = -1.0

	
}

void function ServerCallback_SquadRequeueOptIn( entity optInPlayer, float optInStartTime )
{
	if ( !( IsValid( optInPlayer ) && optInPlayer.IsConnectionActive() ) )
		return

	EHI optInEHI = ToEHI( optInPlayer )
	if ( clFile.squadOptIns.find( optInEHI ) != -1 )
		return

	entity player = GetLocalClientPlayer()
	if ( optInPlayer == player )
	{
		clFile.amIOptedIn = true
		clFile.partyUpdateCallbackAdded = true
		AddCallback_OnPartyUpdated( OnPartyUpdated_AfterOptIn )
	}
	else
	{
		
		entity ornull leaderPlayerOrNull = ClGetLeaderIfPartyMembersInSquad()
		if ( leaderPlayerOrNull != null )
		{
			entity leaderPlayer = expect entity( leaderPlayerOrNull )
			if ( leaderPlayer.GetUserID() == optInPlayer.GetUserID() )
				ClSquadRequeue_OptIn()
		}
	}

	clFile.squadOptIns.append( optInEHI )
	clFile.optInStartTime = optInStartTime
	

	
}

void function ServerCallback_SquadRequeuePartyMerge( EHI leaderPlayerEHI, bool success )
{
	if ( success && leaderPlayerEHI == LocalClientEHI() )
	{
		if ( leaderPlayerEHI == LocalClientEHI() )
		{
			printt( "ServerCallback_SquadRequeuePartyMerge success!" )

			
			
			RunUIScript( "StartMatchmakingFromMatch" )
		}
		else
		{
			entity leaderPlayer = FromEHI( leaderPlayerEHI )
			if ( IsValid( leaderPlayer ) && leaderPlayer.IsConnectionActive() )
			{
				
			}
			else
			{
				
			}
		}
	}
	else
	{
		
	}

	if ( clFile.partyUpdateCallbackAdded )
	{
		clFile.partyUpdateCallbackAdded = false
		RemoveCallback_OnPartyUpdated( OnPartyUpdated_AfterOptIn )
	}
}


bool function ClSquadRequeue_OptIn()
{
	if ( clFile.amIOptedIn )
		return false

	entity localPlayer = GetLocalClientPlayer()
	if ( !IsMatchmakingFromMatchAllowed( localPlayer ) )
		return false

	if ( !GetConVarBool( "matchSquadRequeue_enabled" ) )
		return false

	entity ornull partyLeaderPlayerOrNull = ClGetLeaderIfPartyMembersInSquad()
	if ( partyLeaderPlayerOrNull == null )
		return false

	entity partyLeaderPlayer = expect entity( partyLeaderPlayerOrNull )
	clFile.optInLeaderUid = partyLeaderPlayer.GetUserID()
	Remote_ServerCallFunction( "ClientCallback_SquadRequeueOptIn", partyLeaderPlayer )
	return true
}

bool function ClSquadRequeue_OptOut()
{
	if ( !clFile.amIOptedIn )
		return false

	entity localPlayer = GetLocalClientPlayer()
	if ( !IsMatchmakingFromMatchAllowed( localPlayer ) )
		return false

	if ( !GetConVarBool( "matchSquadRequeue_enabled" ) )
		return false

	if ( clFile.partyUpdateCallbackAdded )
	{
		clFile.partyUpdateCallbackAdded = false
		RemoveCallback_OnPartyUpdated( OnPartyUpdated_AfterOptIn )
	}

	Remote_ServerCallFunction( "ClientCallback_SquadRequeueOptOut" )
	return true
}



































































































































































































































