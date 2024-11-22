 //Code-in-X++-for-Membership-Management


class MembershipManager {
    public static void addMembership(str membershipNumber, int durationInMonths) {
        LibraryMembershipTable membership;
        
        membership.MembershipNumber = membershipNumber;
        membership.StartDate = DateTimeUtil::utcNow();
        membership.ExpiryDate = DateTimeUtil::addMonths(membership.StartDate, durationInMonths);
        
        // Insert into table
        membership.insert();
    }

    public static void updateMembership(str membershipNumber, int extendMonths) {
        LibraryMembershipTable membership;
        
        select forUpdate membership
        where membership.MembershipNumber == membershipNumber;

        if (membership.RecId) {
            membership.ExpiryDate = DateTimeUtil::addMonths(membership.ExpiryDate, extendMonths);
            membership.update();
        } else {
            throw error("Membership not found.");
        }
    }
}
