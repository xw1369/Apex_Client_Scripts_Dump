










const int TEAM_NAME_MIN_LENGTH = 3

struct PlacementStruct
{
	var           headerPanel
	var           listPanel
	int           teamIndex
	int			  teamPlacement
	int           teamSize
	int           teamDisplayNumber

	array<var>      _listButtons

	array<PrivateMatchStatsStruct> playerPlacementData
}

enum ePlacementSortingMethod
{
	BY_PLACEMENT,
	BY_TEAM_INDEX,

	_count
}

struct
{
	var menu

	var continueButton

	var decorationRui
	var menuHeaderRui

	var teamRosterPanel

	bool wasPartyMember = false 
	bool disableNavigateBack = false

	bool skippableWaitSkipped = false

	int xpChallengeTier = -1
	int xpChallengeValue = -1

	var    setTeamNameDialog
	int    setTeamNameDialog_teamIndex
	var    TextEntryCodeBox
	string setTeamNameDialog_teamName

	table< int, PlacementStruct > teamPlacement

	var sortingMethodButton
	int sortingMethod = ePlacementSortingMethod.BY_PLACEMENT
} file





















































































































































